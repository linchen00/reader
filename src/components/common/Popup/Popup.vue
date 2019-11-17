<template>
  <div class="popup" v-show="popupVisible">
    <transition name="fade">
      <div class="popup-bg" @click.stop.prevent="hide"></div>
    </transition>
    <transition name="popup-slide-up">
      <div class="popup-wrapper" v-show="visible">
        <div class="popup-title" v-if="title && title.length>0">{{title}}</div>
        <div class="popup-btn"
             :class="{'danger':item.type==='danger'}"
             v-for="(item,index) in btn"
             :key="index"
             @click="item.click">{{item.text}}
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  export default {
    name: "Popup",
    props: {
      title: String,
      btn: Array
    },
    data() {
      return {
        popupVisible: false,
        visible: false
      }
    },
    methods: {
      show() {
        this.popupVisible = true
        setTimeout(() => {
          this.visible = true
        })
      },
      hide() {
        this.visible = false
        setTimeout(() => {
          this.popupVisible = false
        }, 200)

      }
    }
  }
</script>

<style scoped>
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    z-index: 1000;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .4);
  }

  .popup-bg {
    height: 100%;
    width: 100%;
  }

  .popup-wrapper {
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 1000;
    width: 100%;
    background: #fff;
  }

  .popup-title {
    width: 100%;
    height: 44px;
    border-bottom: 1px solid #eee;
    font-size: 12px;
    line-height: 14px;
    padding: 15px;
    box-sizing: border-box;
    text-align: center;
    color: #999;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .popup-btn {
    width: 100%;
    height: 60px;
    border-bottom: 1px solid #eee;
    font-size: 16px;
    color: #666;
    font-weight: bold;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .popup-btn.danger {
    color: rgb(255, 102, 159);
  }
</style>
