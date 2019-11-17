<template>
  <transition name="slide-down">
    <div class="title-wrapper" v-show="menuVisible">
      <div class="left">
        <span class="font_family">&#xe600;</span>
      </div>
      <div class="right">
        <div class="icon-wrapper">
          <span class="font_family">&#xe618;</span>
          <span class="font_family">&#xe60e;</span>
          <span class="font_family">&#xe61d;</span>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  import {ebookMixin} from "utils/mixin";

  export default {
    name: "EbookTitle",
    mixins: [ebookMixin]
  }
</script>

<style scoped>
  .title-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 49px;
    z-index: 999;
    width: 100%;
    display: flex;
    align-items: center;
    background: #7b7f97;
    box-shadow: 0 10px 40px #1d2328;
  }

  .title-wrapper .left {
    flex: 1;
    text-align: left;
  }

  .title-wrapper .right {
    flex: 1;
    text-align: right;
  }

  .title-wrapper .right span {
    padding: 0 5px;
  }


</style>
