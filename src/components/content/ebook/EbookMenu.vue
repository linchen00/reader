<template>
  <div>
    <div class="menu-wrapper" v-show="menuVisible" :class="{'hide-box-shadow':settingShadowVisible}">
      <div class="icon-wrapper">
        <span class="font_family" @click="showSeting(4)">&#xe613;</span>
      </div>
      <div class="icon-wrapper" @click="showSeting(3)">
        <span class="font_family">&#xe60a;</span>
      </div>
      <div class="icon-wrapper" @click="showSeting(2)">
        <span class="font_family">&#xe61e;</span>
      </div>
      <div class="icon-wrapper" @click="showSeting(0)">
        <span class="font_family">&#xe60b;</span>
      </div>
    </div>
    <ebook-setting-font></ebook-setting-font>
    <ebook-setting-font-popup></ebook-setting-font-popup>
    <ebook-setting-theme></ebook-setting-theme>
    <ebook-setting-progress></ebook-setting-progress>
    <ebook-slide></ebook-slide>
  </div>
</template>

<script>
  import EbookSlide from "./EbookSlide";
  import EbookSettingFont from "./EbookSettingFont";
  import EbookSettingFontPopup from "./EbookSettingFontPopup";
  import EbookSettingTheme from "./EbookSettingTheme";
  import EbookSettingProgress from "./EbookSettingProgress";

  import {ebookMixin} from "utils/mixin";

  export default {
    name: "EbookMenu",
    mixins: [ebookMixin],
    components: {
      EbookSettingFont,
      EbookSettingFontPopup,
      EbookSettingTheme,
      EbookSettingProgress,
      EbookSlide
    },
    methods: {
      showSeting(key) {
        if (key !== this.settingVisible) {
          this.setSettingVisible(key)
          this.setSettingShadowVisible(true)
        } else {
          this.setSettingVisible(-1)
          this.setSettingShadowVisible(false)
        }
      }
    }
  }
</script>


<style scoped lang="scss" rel="stylesheet/scss">
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    height: 49px;
    width: 100%;
    background: #7b7f97;
    justify-content: center;
    align-items: center;
    box-shadow: 0 10px 40px #1d2328;
    z-index: 1000;
  }

  .hide-box-shadow {
    box-shadow: none;
  }

  .menu-wrapper .icon-wrapper {
    flex: 1;
    text-align: center;
    font-size: 26px;
  }

  .font_family {
    font-size: 26px;
  }
</style>
