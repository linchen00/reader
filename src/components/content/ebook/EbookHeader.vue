<template>
  <div class="ebook-header">
    <span class="ebook-header-text">{{getSectionName}}</span>
  </div>
</template>

<script>
  import {ebookMixin} from 'utils/mixin'

  export default {
    name: "EbookHeader",
    mixins: [ebookMixin]
  }
</script>

<style  scoped>

  .ebook-header {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    height: 48px;
    padding: 0 15px;
    box-sizing: border-box;
    overflow: hidden;
    display: flex;
    justify-content: left;
    align-items: center;
  }

  .ebook-header-text {
    font-size: 12px;
    color: #6d7178;
  }
</style>


