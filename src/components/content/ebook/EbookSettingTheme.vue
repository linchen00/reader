<template>
  <div class="setting-wrapper" v-show="settingVisible === 2">
    <div class="setting-theme" >
      <div class="settingg-theme-item" v-for="(item, index) in themeList" :key="index" @click="setTheme(index)">
        <div class="preview" :style="{background: item.style.body.background}" :class="{select:item.name === defaultTheme}"></div>
        <div class="text" :class="{select: item.name === defaultTheme}">{{item.name}}</div>
      </div>
    </div>
  </div>
</template>

<script>
  import {ebookMixin} from "utils/mixin";
  import {saveTheme} from "../../../utils/localStorage";

  export default {
    name: "EbookSettingTheme",
    mixins:[ebookMixin],
    methods:{
      setTheme(index){
        const theme = this.themeList[index]
        this.setDefaultTheme(theme.name).then(() =>{
          this.currentBook.rendition.themes.select(this.defaultTheme)
        })
        this.initGlobalStyle()
        saveTheme(this.fileName,theme.name)
      }
    }
  }
</script>

<style scoped>
  .setting-wrapper {
    position: absolute;
    bottom: 49px;
    left: 0;
    right: 0;
    width: 100%;
    height: 90px;
    z-index: 999;
    background: #7b7f97;
    box-shadow: 0 10px 40px #1d2328;
  }

  .setting-theme {
    display: flex;
    height: 100%;
  }

  .settingg-theme-item {
    display: flex;
    flex: 1;
    flex-direction: column;
    padding: 15px;
    box-sizing: border-box;
  }

  .preview {
    flex: 1;
    background: #55a532;
    box-sizing: border-box;
  }
  .preview.select{
    border: 1px solid #f54d82;
  }

  .text {
    margin-top:5px;
    flex: 0 0 20px;
    font-size: 16px;
    align-items: center;
    justify-content: center;
    text-align: center;
  }
  .text.select{
    color: #f54d82;
  }
</style>
