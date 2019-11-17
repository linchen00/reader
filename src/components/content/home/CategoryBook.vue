<template>
  <div class="category-book">
    <title-view :label="categoryText(data.category)" btn="查看全部" @onClick="showBookCategory"></title-view>
    <div class="category-book-list">
      <div class="category-book-item" v-for="(item, index) in data.list" :key="index" @click="showBookDetail(item)">
        <div class="img-wrapper">
          <img class="img" :src="item.cover">
        </div>
        <div class="content-wrapper">
          <div class="title title-small" ref="title">{{item.title}}</div>
          <div class="num sub-title-tiny" ref="author">{{item.author}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script >
  import TitleView from './Title'
  import {categoryText,getCategoryName} from "../../../utils/store";
  import {storeHomeMixin} from "../../../utils/mixin";

  export default {
    mixins: [storeHomeMixin],
    components: {
      TitleView
    },
    props: {
      data: Object
    },
    methods: {
      showBookCategory() {
        this.$router.push({
          path: '/store/list',
          query: {
            category: getCategoryName(this.data.category),
            categoryText: this.categoryText(this.data.category)
          }
        })
      },
      categoryText(category) {
        return categoryText(category)
      }
    }
  }
</script>

<style scoped>

  .category-book-list {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    padding: 0 5px;
    box-sizing: border-box;
  }

  .category-book-item {
    flex: 0 0 25%;
    width: 25%;
    padding: 0 5px;
    box-sizing: border-box;
  }

  .img-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .img {
    width: 100%;
  }

  .content-wrapper {
    width: 100%;
    margin-top: 10px;

  }

  .num {
    margin-top: 5px;
  }
</style>
