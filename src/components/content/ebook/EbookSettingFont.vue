<template>
  <transition>
    <div class="setting-wrapper" v-show="menuVisible && (settingVisible === 0 | settingVisible === 1)">
      <div class="setting-font-size">
        <div class="preview" :style="{ fontSize: fontSizeList[0].fontSize + 'px' }">A</div>
        <div class="select-wrapper" v-for="(item,index) in fontSizeList" :key="index"
             @click="setFontSize(item.fontSize)">
          <div class="line"></div>
          <div class="short-line"></div>
          <div class="point" v-show="defaultFontSize === item.fontSize"></div>
        </div>
        <div class="preview" :style="{fontSize: fontSizeList[fontSizeList.length - 1].fontSize+'px' }">A</div>
      </div>
      <div class="setting-font-family" @click="showFontFamilyPopup">
        <div class="font-family-text-wrapper">
          <span class="font-family-text" >{{defaultFontFamily}}</span>
        </div>
        <div class="font-family-icon-wrapper">
          <span class="font-family-icon font_family">&#xe603;</span>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  import {ebookMixin} from "utils/mixin";
  import {FONT_SIZE_LIST} from "utils/book";
  import {saveFontSize} from "utils/localStorage";

  export default {
    name: "EbookSettingFont",
    mixins: [ebookMixin],
    data() {
      return {
        fontSizeList: FONT_SIZE_LIST
      }
    },
    methods: {
      setFontSize(fontSize) {
        this.setDefaultFontSize(fontSize)
        saveFontSize(this.filename, fontSize)
        this.currentBook.rendition.themes.fontSize(fontSize)
      },
      showFontFamilyPopup(){
        this.setSettingVisible(1)
      }
    },
  }
</script>

<style scoped>
  .setting-wrapper {
    position: absolute;
    bottom: 49px;
    left: 0;
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 90px;
    box-shadow: 0 10px 40px #1d2328;
    background: #7b7f97;
    z-index: 999;
  }

  .setting-font-size {
    flex: 1;
    display: flex;
    height: 100%;

  }

  .preview {
    flex: 0 0 40px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .select-wrapper {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
  }

  .select-wrapper .short-line {
    width: 1px;
    height: 8px;
    background: #ddd;
  }

  .select-wrapper .line {
    height: 0;
    flex: 1;
    border-top: 1px solid #ddd;
  }

  .select-wrapper:nth-child(2) {
    flex: 0 0 0;
  }

  .select-wrapper .point {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: #fff;
    border: 1px solid #ccc;
    box-shadow: 0 0 1px rgba(0, 0, 0, 0.15);
    position: absolute;
    right: -11px;
    z-index: 999;
  }


  .setting-font-family {
    flex: 1;
    display: flex;
    font-size: 16px;
    justify-content: center;
    align-items: center;
  }

  .font-family-text-wrapper, .font-family-icon-wrapper {
    justify-content: center;
    align-items: center;
  }

  .font-family-icon {
    font-size: 16px;
  }
</style>
