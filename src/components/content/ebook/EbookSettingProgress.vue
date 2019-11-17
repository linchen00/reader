<template>
  <div class="setting-wrapper" v-show="menuVisible && settingVisible === 3">
    <div class="setting-progress">
      <div class="read-time-wrapper">
        <span class="read-time">已读{{getReadTimeText()}}</span>
        <span class="font_family">&#xe603;</span>
      </div>
      <div class="progress-wrapper">
        <span class="font_family" @click="prevSection">&#xe600;</span>
        <input type="range"
               max="100"
               min="0"
               @change="onProgressChange($event.target.value)"
               @input="onProgressInput($event.target.value)"
               step="1"
               :disabled="!bookAvailable"
               ref="progress"
               :value="progress">
        <span class="font_family" @click="nextSection">&#xe603;</span>
      </div>
      <div class="text-wrapper">
        <span class="text-section">{{getSectionName}}</span>
        <span>({{bookAvailable ? progress + '%' : '加载中...'}})</span>
      </div>
    </div>
  </div>
</template>

<script>
  import {ebookMixin} from "utils/mixin";

  export default {
    name: "EbookSettingProgress",
    mixins: [ebookMixin],
    methods: {
      onProgressChange(progress) {
        this.setProgress(progress).then(() => {
          this.displayProgress()
          this.updateProgress()
        })
      },
      displayProgress() {
        const cfi = this.currentBook.locations.cfiFromPercentage(this.progress / 100);
        this.display(cfi)
      },
      onProgressInput(progress) {
        this.setProgress(progress).then(() => {
          this.updateProgress()
        })

      },
      updateProgress() {
        this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
      },
      prevSection() {
        if (this.section > 0 && this.bookAvailable) {
          this.setSection(this.section - 1).then(() => {
            this.displaySection()
          })
        }
      },
      nextSection() {
        if (this.section < this.currentBook.spine.length - 1 && this.bookAvailable) {
          this.setSection(this.section + 1).then(() => {
            this.displaySection()
          })
        }
      },
      displaySection() {
        const sectionInfo = this.currentBook.section(this.section)
        if (sectionInfo && sectionInfo.href) {
          this.display(sectionInfo.href)
        }
      },
    },
    updated() {
      this.updateProgress()
    }
  }
</script>

<style scoped>
  .setting-progress {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 90px;
    position: absolute;
    z-index: 999;
    bottom: 49px;
    left: 0;
    right: 0;
    background: #7b7f97;
    box-shadow: 0 10px 40px #1d2328;
  }

  .read-time-wrapper, .text-wrapper {
    display: flex;
    flex: 1;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 10px;
    font-size: 14px;
  }

  .read-time-wrapper .font_family {
    font-size: 14px;
  }

  .progress-wrapper {
    display: flex;
    flex: 2;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .progress-wrapper input[type="range"] {
    width: 100%;
    -webkit-appearance: none;
    height: 2px;
    margin: 10px 0;
    border-radius: 2px;
    background: -webkit-linear-gradient(#7b7f97, #7b7f97) no-repeat, #ddd;
  }

  .progress-wrapper input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    height: 20px;
    width: 20px;
    border-radius: 50%;
    background: white;
    box-shadow: 0 4px 4px 0 rgba(0, 0, 0, .15);
    border: 1px solid #ddd;
  }

  .progress-wrapper span {
    padding: 0 15px;
  }

  .text-wrapper .text-section {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }

</style>
