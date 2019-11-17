<template>
  <div class="ebook-slide-bookmark">
    <div class="slide-bookmark-title">书签 · {{bookmark ? bookmark.length : 0}}</div>
    <scroll class="slide-bookmark-list" :top="48" :bottom="48">
      <div class="slide-bookmark-item" v-for="(item, index) in bookmark" :key="index"
           @click="displayBookmark(item.cfi)">
        <div class="slide-bookmark-item-icon">
          <div class="font_family">&#xe615;</div>
        </div>
        <div class="slide-bookmark-item-text">{{item.text}}</div>
      </div>
    </scroll>
  </div>
</template>

<script type="text/ecmascript-6">
  import Scroll from "components/common/scroll/Scroll";
  import {getBookmark} from 'utils/localStorage'
  import {ebookMixin} from 'utils/mixin'

  export default {
    mixins: [ebookMixin],
    components: {
      Scroll
    },
    data() {
      return {
        bookmark: null
      }
    },
    methods: {
      displayBookmark(target) {
        this.display(target, () => {
          this.hideTitleAndMenu()
        })
      }
    },
    mounted() {
      this.bookmark = getBookmark(this.fileName)
    }
  }
</script>

<style scoped>


  .ebook-slide-bookmark {
    width: 100%;
  }

  .slide-bookmark-title {
    width: 100%;
    height: 48px;
    font-size: 14px;
    font-weight: bold;
    padding: 0 15px;
    box-sizing: border-box;
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }

  .slide-bookmark-list {
    padding: 0 15px;
    box-sizing: border-box;

  }

  .slide-bookmark-item {
    display: flex;
    padding: 15px 0;
    box-sizing: border-box;
  }

  .slide-bookmark-item-icon {
    flex: 0 0 29px;
    display: flex;
    justify-content: flex-start;
    align-items: center;

  }

  .font_family {
    flex: 0 0 20px;
    width: 20px;
    height: 20px;
    font-size: 12px;
    border-radius: 50%;
    background: #bbb;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .slide-bookmark-item-text {
    font-size: 14px;
    line-height: 15px;
    max-height: 45px;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
    white-space: normal;
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: keep-all;
  }
</style>
