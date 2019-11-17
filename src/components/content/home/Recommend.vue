<template>
  <div class="recommend">
    <title-view label="热门推荐" btn="查看全部"></title-view>
    <div class="recommend-list">
      <div class="recommend-item" v-for="(item, index) in data" :key="index" @click="showBookDetail(item)">
        <div class="img-wrapper">
          <img class="img" :src="item.cover">
        </div>
        <div class="content-wrapper">
          <div class="title title-medium" ref="title">{{item.title}}</div>
          <div class="num sub-title" ref="num">{{'$1人同时在读'.replace('$1', item.readers)}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import TitleView from './Title'
  import {storeHomeMixin} from 'utils/mixin'

  export default {
    mixins: [storeHomeMixin],
    components: {
      TitleView
    },
    props: {
      data: Array
    }
  }
</script>

<style scoped>
  .recommend-list {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    padding: 0 5px;
    box-sizing: border-box;

  }

  .recommend-item {
    flex: 0 0 33.33%;
    width: 33.33%;
    padding: 0 5px;
    box-sizing: border-box;
  }

  .img-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .img {
    width: 80%;
  }

  .content-wrapper {
    width: 100%;
    margin-top: 10px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;;
  }

  .title {
    text-align: center;
  }

  .num {
    margin-top: 5px;
  }
</style>
