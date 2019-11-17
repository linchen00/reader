<template>
  <div class="setting-wrapper" v-show="settingVisible === 1">
    <div class="ebook-popup-list" >
      <div class="ebook-popup-title">
        <div class="ebook-popup-title-icon" @click="hide">
          <span class="font_family">&#xe622;</span>
        </div>
        <span class="ebook-popup-title-text">选择字体</span>
      </div>
      <div class="ebook-popup-list-wrapper">
        <div class="ebook-popup-item" v-for="(item,index) in fontFamilyList" :key="index"
             @click="setFontFamily(item.font)">
          <div class="ebook-popup-item-text " :class="{select:isSelect(item.font)}">{{item.font}}</div>
          <div class="ebook-popup-item-check">
            <span class="font_family" v-if="isSelect(item.font)">&#xe602;</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {FONT_FAMILY} from "../../../utils/book";
  import {ebookMixin} from "utils/mixin";
  import {saveFontFamily} from "utils/localStorage";


  export default {
    mixins: [ebookMixin],
    data() {
      return {
        fontFamilyList: FONT_FAMILY
      }
    },
    methods: {
      hide() {
        this.setSettingVisible(0)
      },
      setFontFamily(font) {
        this.setDefaultFontFamiy(font)
        saveFontFamily(this.filename, font)
        if (font === 'Default') {
          this.currentBook.rendition.themes.font('Times New Roman')
        } else {
          this.currentBook.rendition.themes.font(font)
        }
      },
      isSelect(font) {
        return this.defaultFontFamily === font

      }
    }


  }
</script>

<style scoped>
  .setting-wrapper{
    width: 100%;
    height: 300px;
    position: absolute;
    bottom: 0;
    right: 0;
    left: 0;
    z-index: 999;
  }
  .ebook-popup-list {
    width: 100%;
    background: 100%;
    background: #fff;
  }

  .ebook-popup-title {
    position: relative;
    padding: 15px;
    box-sizing: border-box;
    border-bottom: 1px solid #b8b9bb;
    text-align: center;
  }

  .ebook-popup-title-icon {
    position: absolute;
    left: 15px;
    top: 0;
    height: 100%;
    font-size: 16px;
    font-weight: bold;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .ebook-popup-list-wrapper {
    height: 100%;
  }

  .ebook-popup-item {
    display: flex;
    padding: 15px;
    align-items: center;
  }

  .ebook-popup-item-text {
    flex: 1;
    text-align: left;
    font-size: 16px;
  }

  .ebook-popup-item-check {
    flex: 1;
    text-align: right;

  }

  .ebook-popup-item-check .font_family {
    color: #346cb9;
    font-size: 14px;
    font-weight: bold;
  }

  .select {
    color: #346cb9;
    font-weight: bold;
  }
</style>
