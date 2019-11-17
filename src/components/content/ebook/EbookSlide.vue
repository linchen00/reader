<template>
  <div class="slide-content-wrapper" v-show="menuVisible && (settingVisible === 4)">
    <div class="content">
      <div class="content-page-wrapper" v-if="bookAvailable">
        <div class="content-page">
          <component :is="currentTab === 1 ? content : bookmark"></component>
        </div>
        <div class="content-page-tab">
          <div class="content-page-tab-item" :class="{'select':currentTab === 1}" @click="selectTab(1)">目录</div>
          <div class="content-page-tab-item" :class="{'select':currentTab === 2}" @click="selectTab(2)">书签</div>
        </div>
      </div>
      <div class="content-empty" v-else>
        <ebook-loading></ebook-loading>
      </div>
    </div>
    <div class="content-bg" @click="hideTitleAndMenu"></div>
  </div>
</template>

<script>
  import EbookSlideBookmark from "./EbookSlideBookmark";
  import EbookSlideContent from "./EbookSlideContent";
  import EbookLoading from "./EbookLoading";
  import {ebookMixin} from "../../../utils/mixin";

  export default {
    name: "EbookSlide",
    components:{
      EbookLoading
    },
    data() {
      return {
        currentTab: 1,
        content:EbookSlideContent,
        bookmark:EbookSlideBookmark
      }
    },
    mixins: [ebookMixin],
    methods: {
      selectTab(currentTab) {
        this.currentTab = currentTab
      }
    }
  }
</script>

<style scoped>
  .slide-content-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1001;
    display: flex;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.15);
  }

  .content {
    flex: 0 0 85%;
    width: 85%;
    height: 100%;
    background: #7b7f97;
  }

  .content-page-wrapper {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
  }

  .content-page {
    flex: 1;
    width: 100%;
    overflow: hidden;
  }

  .content-page-tab {
    width: 100%;
    height: 49px;
    display: flex;
    flex: 0 0 49px;
  }

  .content-page-tab-item {
    display: flex;
    flex: 1;
    font-size: 16px;
    align-items: center;
    justify-content: center;
  }

  .content-bg {
    flex: 0 0 15%;
    width: 15%;
    height: 100%;
  }

  .select {
    color: #9a6e3a;
    font-weight: bold;
  }
  .content-empty{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
