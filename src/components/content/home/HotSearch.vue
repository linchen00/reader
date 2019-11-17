<template>
  <div>
    <div class="hot-search-title">
      <span class="label">{{label}}</span>
      <span class="btn">{{btn}}</span>
    </div>
    <div class="hot-search-list">
      <div class="hot-search-item" v-for="(item, index) in hotSearch" :key="index">
        <div class="icon-wrapper">
          <span class="font_family " v-if="item.type === 1">&#xe624;</span>
          <span class="font_family " v-if="item.type === 2">&#xe621;</span>
        </div>
        <div class="hot-search-text-wrapper">
          <div class="text" ref="searchText">{{item.text}}</div>
          <div class="num" v-if="item.num">{{item.num}}人搜索</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">


  export default {
    props: {
      label: String,
      btn: String,
      hotSearch: Array
    },
    mounted() {
      this.$refs.searchText.forEach(item => {
        item.style.width = window.innerWidth - 20 - 40 + 'px'
      })
    }
  }
</script>

<style scoped>

  .hot-search-title {
    display: flex;
    width: 100%;
    font-size: 13px;
    padding: 10px;
    box-sizing: border-box;
  }

  .label {
    flex: 1;
    text-align: left;
    font-weight: bold;
    color: #666;
  }

  .btn {
    flex: 1;
    text-align: right;
    font-weight: bold;
    color: #409EFF;
  }

  .hot-search-list {
    width: 100%;
    padding: 0 10px;
    box-sizing: border-box;

  }

  .hot-search-item {
    padding: 10px 0;
    display: flex;


  }

  .icon-wrapper {
    flex: 0 0 40px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .font_family {
    font-size: 16px;
    color: #666;
  }


  .hot-search-text-wrapper {
    flex: 1;
    height: 35px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
  }

  .text {
    flex: 1;
    height: 100%;
    font-size: 14px;
    font-weight: bold;
    color: #666;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }

  .num {
    font-size: 12px;
    color: #999;
    margin-top: 5px;
  }
</style>
