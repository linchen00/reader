<template>
  <div class="ebook-slide-content">
    <div class="slide-content-search-wrapper">
      <div class="slide-content-search-input-wrapper">
        <div class="slide-content-search-icon">
          <span class="font_family">&#xe621;</span>
        </div>
        <input type="text"
               class="slide-content-search-input"
               @click="showSearchCancel"
               v-model="searchText"
               @keyup.enter.exact="search()">
      </div>
      <div class="slide-content-search-cancel" v-if="searchVisible" @click="hideSearchCancel">取消</div>
    </div>
    <div class="slide-content-book-wrapper" v-show="!searchVisible">
      <div class="slide-content-book-img-wrapper">
        <img :src="cover" alt="" class="slide-content-book-img">
      </div>
      <div class="slide-content-book-info-wrapper">
        <div class="slide-content-book-title">
          <span class="slide-content-book-title-text">{{title}}</span>
        </div>
        <div class="slide-content-book-author">
          <span class="slide-content-book-author-text">{{author}}</span></div>
      </div>
      <div class="slide-content-book-progress-wrapper">
        <div class="slide-content-book-progress">
          <span class="progress">{{progress + '%'}}已读</span>
          <div class="pregress-text">已读{{getReadTimeText()}}</div>
        </div>
      </div>
    </div>
    <scroll class="slide-contents-list" :top="181" :bottom="49" v-show="!searchVisible">
      <div class="slide-content-item" v-for="(item,index) in navigation" :key="index">
        <span class="slide-content-item-label"
              :style="contentItemStyle(item)"
              :class="{'select':section === index}"
              @click="displayContent(item.href)">{{item.label}}</span>
        <span class="slide-content-item-page">{{item.page}}</span>
      </div>
    </scroll>
    <scroll :top="66" :bottom="49" v-show="searchVisible">
      <div class="slide-search-list" v-for="(item,index) in searchList" :key="index">
        <div class="slide-search-item"  v-html="item.excerpt" @click="displayContent(item.cfi,true)"></div>
      </div>
    </scroll>
  </div>
</template>

<script>
  import {ebookMixin} from "utils/mixin";
  import Scroll from "components/common/scroll/Scroll";

  export default {
    name: "EbookSlideContent",
    mixins: [ebookMixin],
    components: {
      Scroll
    },
    computed: {
      title() {
        if (this.metadata) {
          return this.metadata.title
        }
      },
      author() {
        if (this.metadata) {
          return this.metadata.creator
        }
      },

    },
    data() {
      return {
        searchVisible: false,
        searchList: [],
        searchText: ''
      }
    },
    methods: {
      displayContent(target,hightLight=false){
        this.display(target,()=>{
          this.hideTitleAndMenu()
          if (hightLight){
            this.currentBook.rendition.annotations.highlight(target)
          }
        })
      },
      search() {
        if (this.searchText && this.searchText.length > 0) {
          this.doSearch(this.searchText).then(list => {
            this.searchList = list
            this.searchList.map(item => {
              item.excerpt = item.excerpt.replace(this.searchText, `<span class="content-search-text">${this.searchText}</span>`)
              return item
            })
          })
        }
      },
      doSearch(q) {
        return Promise.all(
            this.currentBook.spine.spineItems.map(
              section => section.load(this.currentBook.load.bind(this.currentBook))
                .then(section.find.bind(section, q))
                .finally(section.unload.bind(section)))
        ).then(results => Promise.resolve([].concat.apply([], results)))
      },
      hideSearchCancel() {
        this.searchVisible = false
      },
      showSearchCancel() {
        this.searchVisible = true
      },
      contentItemStyle(item) {
        return {
          marginLeft: item.level * 15 + 'px'
        }
      }
    },
  }
</script>

<style scoped>
  .ebook-slide-content {
    width: 100%;
  }

  .slide-content-search-wrapper {
    width: 100%;
    height: 36px;
    display: flex;
    margin: 20px 0 10px 0;
    padding: 0 15px;
    box-sizing: border-box;
  }

  .slide-content-search-input-wrapper {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid #ccc;
  }

  .slide-content-search-icon {
    flex: 0 0 28px;

    display: flex;
    justify-content: center;
    align-items: center;
  }

  .slide-content-search-icon .font_family {
    font-size: 18px;
  }

  .slide-content-search-input {
    flex: 1;
    width: 100%;
    height: 32px;
    border: none;
    font-size: 14px;
    background: transparent;
  }

  .slide-content-search-input:focus {
    outline: none;
  }

  .slide-content-search-cancel {
    display: flex;
    flex: 0 0 45px;
    font-size: 14px;
    justify-content: flex-end;
    align-items: center;
  }

  .slide-content-book-wrapper {
    width: 100%;
    display: flex;
    height: 90px;
    padding: 15px 20px 10px 15px;
    box-sizing: border-box;
  }

  .slide-content-book-img-wrapper {
    flex: 0 0 45px;
  }

  .slide-content-book-img {
    width: 45px;
    height: 60px;
  }

  .slide-content-book-info-wrapper {
    flex: 1;
    padding: 0 10px;
    flex-direction: column;
  }

  .slide-content-book-title {
    font-size: 14px;
    line-height: 16px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }

  .slide-content-book-title-text {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
    white-space: normal;
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: keep-all;
  }

  .slide-content-book-author {
    margin-top: 5px;
    font-size: 12px;
    line-height: 16px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }

  .slide-content-book-author-text {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 1;
    white-space: normal;
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: keep-all;
  }

  .slide-content-book-progress-wrapper {
    display: flex;
    flex: 0 0 90px;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .progress {
    font-size: 14px;
  }

  .pregress-text {
    font-size: 12px;
  }

  .slide-contents-list {
    padding: 0 15px;
    box-sizing: border-box;
  }

  .slide-content-item {
    display: flex;
    padding: 20px 0;
    box-sizing: border-box;
  }

  .slide-content-item-label {
    flex: 1;
    font-size: 14px;
    line-height: 16px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .slide-content-item-page {
    flex: 0 0 30px;
    font-size: 10px;
    display: flex;
    justify-content: right;
    align-items: center;
  }

  .slide-search-list {
    width: 100%;
    padding: 0 15px;
    box-sizing: border-box;
  }

  .slide-search-item {
    font-size: 14px;
    line-height: 16px;
    padding: 20px 0;
    box-sizing: border-box;
  }


  .select {
    color: #9a6e3a;
  }
</style>
