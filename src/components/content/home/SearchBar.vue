<template>
  <div>
    <div class="search-bar" :class="{'hide-title':!titleVisible,'hide-shadow':!shadowVisible}">
    <transition name="title-move">
      <div class="search-bar-title-wrapper" v-show="titleVisible">
        <div class="title-text-wrapper">
          <span class="title-text">书城</span>
        </div>
        <div class="title-icon-shake-wrapper" @click="showFlapCard">
          <span class="font_family">&#xe61b;</span>
        </div>
      </div>
    </transition>
    <div class="title-icon-back-wrapper"
         :class="{'hide-title':!titleVisible}"
    @click="back">
      <span class="font_family">&#xe600;</span>
    </div>
    <div class="search-bar-input-wrapper" :class="{'hide-title':!titleVisible}">
      <div class="search-bar-blank" :class="{'hide-title':!titleVisible}"></div>
      <div class="search-bar-input">
        <span class="font_family">&#xe621;</span>
        <input type="text"
               class="input"
               placeholder="计算机科学和软件工程"
               v-model="searchText"
        @click="showHotSearch">
      </div>
    </div>
  </div>
    <hot-search-list v-show="hotSearchVisible" ref="hotSearch"></hot-search-list>
  </div>

</template>

<script>
  import HotSearchList from "./HotSearchList";
  import {storeHomeMixin} from "utils/mixin";

  export default {
    name: "SearchBar",
    mixins:[storeHomeMixin],
    data(){
      return {
        searchText:'',
        titleVisible:true,
        shadowVisible:false,
        hotSearchVisible:false
      }
    },
    watch:{
      offsetY(offsetY){
        if (offsetY>0){
          this.hideTitle()
          this.showShadow()
        }else {
          this.showTitle()
          this.hideShadow()
        }
      },
      hotSearchOffsetY(offsetY){
        if (offsetY >0){
          this.showShadow()
        }else {
          this.hideShadow()
        }
      }
    },
    components:{
      HotSearchList,
    },
    methods:{
      showFlapCard(){
        this.setFlapCardVisible(true)
      },
      back(){
        this.hideHotSearch()
      },
      hideHotSearch(){
        this.hotSearchVisible = false
        if (this.offsetY > 0) {
          this.hideTitle()
          this.showShadow()
        } else {
          this.showTitle()
          this.hideShadow()
        }
      },
      showHotSearch(){
        this.hideTitle()
        this.hideShadow()
        this.hotSearchVisible = true
        this.$nextTick(()=>{
          this.$refs.hotSearch.reset()
        })
      },
      hideTitle(){
        this.titleVisible=false
      },
      showTitle(){
        this.titleVisible=true
      },
      hideShadow(){
        this.shadowVisible=false
      },
      showShadow(){
        this.shadowVisible=true
      }
    }
  }
</script>

<style scoped>
  .font_family {
    color: #666;
    font-size: 16px;
  }


  .title-text {
    color: #666;
    font-size: 16px;
    font-weight: bold;
  }
  .search-bar{
    position: relative;
    width: 100%;
    height: 94px;
    z-index: 150;
    box-shadow: 0 2px 2px 0 rgba(0,0,0,.1);
  }
  .search-bar.hide-title{
    height: 52px;
  }
  .search-bar.hide-shadow{
    box-shadow: none;
  }
  .search-bar-title-wrapper{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 42px;
  }
  .title-icon-back-wrapper{
    position: absolute;
    top: 0;
    left: 15px;
    height: 42px;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: all .2s linear;
    z-index: 200;
  }
  .title-icon-back-wrapper.hide-title{
    height: 52px;
  }
  .title-text-wrapper{
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 42px;
  }
  .title-icon-shake-wrapper{
    position: absolute;
    top: 0;
    right: 15px;
    height: 42px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .search-bar-input-wrapper{
    display: flex;
    position: absolute;
    left: 0;
    top: 42px;
    width: 100%;
    height: 52px;
    padding: 10px;
    box-sizing: border-box;
    transition: top .2s linear;
  }
  .search-bar-input-wrapper.hide-title{
    top: 0;
  }
  .search-bar-input-wrapper .search-bar-blank{
    flex: 0 0 0;
    width: 0;
    transition:all .2s linear ;
  }
  .search-bar-input-wrapper .hide-title{
    flex: 0 0 31px;
    width: 31px;
  }
  .search-bar-input{
    flex: 1;
    width: 100%;
    background: #f4f4f4;
    border-radius: 20px;
    padding: 5px 15px;
    justify-content: left;
    align-items: center;
    display: flex;
    box-sizing: border-box;
    border: 1px solid #eee;
  }
  .font_family{
    color: #999;
  }
  .input{
    width: 100%;
    height: 22px;
    border: none;
    background: transparent;
    margin-left: 10px;
    font-size: 12px;
    color: #666;
  }
  .input:focus{
    outline: none;
  }
  .input::-webkit-input-placeholder{
    color: #ccc;
  }
</style>
