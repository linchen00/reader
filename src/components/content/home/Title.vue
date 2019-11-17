<template>
  <div class="home-title-wrapper">
    <div class="label">{{label}}</div>
    <div class="btn" @click="click">{{btn}}</div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      label: String,
      btn: String
    },
    methods: {
      click() {
        this.$emit('onClick')
      }
    }
  }
</script>

<style scoped>

  .home-title-wrapper {
    width: 100%;
    padding: 15px 10px;
    box-sizing: border-box;
    display: flex;
    justify-content: center;
    align-items: flex-start;
  }

  .label {
    flex: 1;
    font-size: 18px;
    color: #333;
    font-weight: bold;
  }

  .btn {
    font-size: 14px;
    color: rgba(64, 158, 255, 1);
  }

  .btn:active {
    color: rgba(64, 158, 255, .5);
  }
</style>
