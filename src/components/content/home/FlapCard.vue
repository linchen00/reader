<template>
  <div class="flap-card-wrapper" v-show="flapCardVisible">
    <div class="flap-card-bg" :class="{'animation':runFlapCardAnimation}" v-show="runPointAnimation">
      <div class="flap-card" v-for="(item,index) in flapCardList" :key=index :style="{zIndex:item.zIndex}">
        <div class="flap-card-circle">
          <div class="flap-card-semi-circle flap-card-semi-circle-left" :style="semCircleStyle(item,'left')"
               ref="left"></div>
          <div class="flap-card-semi-circle flap-card-semi-circle-right" :style="semCircleStyle(item,'right')"
               ref="right"></div>
        </div>
      </div>
      <div class="point-wrapper">
        <div class="point" :class="{'animation':runPointAnimation}" v-for="item in pointList" :key="item"></div>
      </div>
    </div>
    <div class="book-card" :class="{'animation':runBookCardAnimation}" v-show="runBookCardAnimation">
      <div class="book-card-wrapper">
        <div class="img-wrapper">
          <img class="img" :src="data ? data.cover : '' " >
        </div>
        <div class="content-wrapper">
          <div class="content-title">{{data ? data.title : ''}}</div>
          <div class="content-author">{{data ? data.author : ''}}</div>
          <div class="content-category"></div>
        </div>
        <div class="read-btn" @click.stop="showBookDetail(data)">立即阅读</div>
      </div>
    </div>
    <div class="close-btn-wrapper" @click="close()">
      <span class="font_family">&#xe612;</span>
    </div>
  </div>
</template>

<script>
  import {storeHomeMixin} from "../../../utils/mixin";
  import {flapCardList} from "../../../utils/store";

  export default {
    name: "FlapCard",
    mixins: [storeHomeMixin],
    data() {
      return {
        flapCardList,
        front: 0,
        back: 1,
        intervalTime: 25,
        runFlapCardAnimation: false,
        runPointAnimation: false,
        runBookCardAnimation:false
      }
    },
    props:{
      data:Object
    },
    methods: {
      close() {
        this.setFlapCardVisible(false)
        this.stopAnimation()
      },
      semCircleStyle(item, dir) {
        return {
          backgroundColor: `rgb(${item.r},${item.g},${item.b})`,
          backgroundSize: item.backgroundSize,
          backgroundImage: dir === 'left' ? item.imgLeft : item.imgRight
        }
      },
      rotate(index, type) {
        const item = this.flapCardList[index]
        let dom
        if (type === 'front') {
          dom = this.$refs.right[index]
        } else {
          dom = this.$refs.left[index]
        }
        dom.style.transform = `rotateY(${item.rotateDegree}deg)`
        dom.style.backgroundColor = `rgb(${item.r}, ${item._g}, ${item.b})`
      },
      flapCardRotate() {
        const frontFlapCard = this.flapCardList[this.front]
        const backFlapCard = this.flapCardList[this.back]
        frontFlapCard.rotateDegree += 10
        frontFlapCard._g -= 5
        backFlapCard.rotateDegree -= 10
        if (backFlapCard.rotateDegree < 90) {
          backFlapCard._g += 5
        }
        if (frontFlapCard.rotateDegree === 90 && backFlapCard.rotateDegree === 90) {
          backFlapCard.zIndex += 2
        }
        this.rotate(this.front, 'front')
        this.rotate(this.back, 'back')
        if (frontFlapCard.rotateDegree === 180 && backFlapCard.rotateDegree === 0) {
          this.next()
        }
      },
      next() {
        const frontFlapCard = this.flapCardList[this.front]
        const backFlapCard = this.flapCardList[this.back]
        frontFlapCard.rotateDegree = 0
        backFlapCard.rotateDegree = 0
        frontFlapCard._g = frontFlapCard.g
        backFlapCard._g = backFlapCard.g
        this.rotate(this.front, 'front')
        this.rotate(this.back, 'back')
        this.front++
        this.back++
        const len = this.flapCardList.length
        if (this.front >= len) {
          this.front = 0
        }
        if (this.back >= len) {
          this.back = 0
        }
        // 动态设置zIndex
        // 100 -> 96
        // 99 -> 100
        // 98 -> 99
        // 97 -> 98
        // 96 -> 97
        // (0 - 1 + 5) % 5 = 4
        // (1 - 1 + 5) % 5 = 0
        this.flapCardList.forEach((item, index) => {
          item.zIndex = 100 - ((index - this.front + len) % len)
        })
        this.prepare()
      },
      prepare() {
        const backFlapCard = this.flapCardList[this.back]
        backFlapCard.rotateDegree = 180
        backFlapCard._g = backFlapCard.g - 5 * 9
        this.rotate(this.back, 'back')
      },
      reset() {
        this.front = 0
        this.back = 1
        this.flapCardList.forEach((item, index) => {
          item.zIndex = 100 - index
          item._g = item.g
          item.rotateDegree = 0
          this.rotate(index, 'front')
          this.rotate(index, 'back')
        })
        this.runBookCardAnimation = false
        this.runFlapCardAnimation = false
        this.runPointAnimation = false
      },
      startFlapCardAnimation() {
        this.prepare()
        this.task = setInterval(() => {
          this.flapCardRotate()
        }, this.intervalTime)
      },
      stopAnimation() {
        if (this.task) {
          clearInterval(this.task)
        }
        if (this.timeOut) {
          clearInterval(this.timeOut)
        }
        if (this.timeOut2) {
          clearInterval(this.timeOut2)
        }
        this.reset()
      },
      runAnimation() {
        this.runFlapCardAnimation = true
        this.timeOut = setTimeout(() => {
          this.startFlapCardAnimation()
          this.startPointAnimation()
        }, 300)
        this.timeOut2=setTimeout(()=>{
          this.stopAnimation()
          this.runBookCardAnimation=true
        },2500)
      },
      startPointAnimation() {
        this.runPointAnimation = true
        setTimeout(()=>{
          this.runPointAnimation =false
        },750)
      }
    },
    watch: {
      flapCardVisible(v) {
        if (v) {
          this.runAnimation()
        }
      }
    },
    created() {
      this.pointList = []
      for (let i = 0; i < 18; i++) {
        this.pointList.push(`point${i}`)
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
  @import "../../../assets/styles/flapCard.scss";

  .flap-card-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 999;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .6);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .close-btn-wrapper {
    position: absolute;
    left: 0;
    bottom: 30px;
    width: 100%;
    z-index: 999;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .close-btn-wrapper .font_family {
    width: 45px;
    height: 45px;
    font-size: 25px;
    border-radius: 50%;
    background: #333;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .flap-card-bg {
    position: relative;
    width: 64px;
    height: 64px;
    border-radius: 5px;
    background: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    transform: scale(0);
    opacity: 0;
  }

  .flap-card-bg.animation {
    animation: flap-card-move .3s ease-in both;
  }

  @keyframes flap-card-move {
    0% {
      transform: scale(0);
      opacity: 0;
    }
    50% {
      transform: scale(1.2);
      opacity: 1;
    }
    75% {
      transform: scale(.9);
      opacity: 1;
    }
    100% {
      transform: scale(1);
      opacity: 1;
    }
  }

  .point-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
    z-index: 999;
    margin: auto;
  }

  .point {
    border-radius: 50%;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
    margin: auto;
  }

  .point.animation {
    @for $i from 1 to length($moves) {
      &:nth-child(#{$i}) {
        @include move($i)
      }
    }
  }

  .flap-card {
    width: 48px;
    height: 48px;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
  }

  .flap-card-circle {
    width: 100%;
    height: 100%;
    display: flex;
  }

  .flap-card-semi-circle {
    flex: 0 0 50%;
    width: 50%;
    height: 100%;
    background-repeat: no-repeat;
    backface-visibility: hidden;
  }

  .flap-card-semi-circle-left {
    border-radius: 24px 0 0 24px;
    background-position: center right;
    transform-origin: right;
  }

  .flap-card-semi-circle-right {
    border-radius: 0 24px 24px 0;
    background-position: center left;
    transform-origin: left;
  }
  .book-card{
    position: relative;
    width: 65%;
    max-width: 400px;
    box-sizing: border-box;
    border-radius: 15px;
    background: #fff;
  }
  .book-card.animation{
    animation: scale 0.3s ease-in both;
  }
  @keyframes scale {
    0%{
      transform: scale(0);
      opacity: 0;
    }
    100%{
      transform: scale(1);
      opacity: 1;
    }
  }
  .book-card-wrapper{
    width: 100%;
    height: 100%;
    margin-bottom: 44px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
  }
  .img-wrapper{
    width: 100%;
    margin-top: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .img{
    width: 90px;
    height: 130px;
  }
  .content-wrapper{
    padding: 0 20px;
    margin-top: 20px;
    font-size: 14px;
  }
  .content-title{
    color: #333;
    font-weight: bold;
    font-size: 18px;
    line-height: 20px;
    max-height: 40px;
    text-align: center;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    white-space: normal;
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: keep-all;
  }
  .content-author {
    margin-top: 10px;
    text-align: center;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    white-space: normal;
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: keep-all;
  }
  .content-category {
    color: #999;
    font-size: 14px;
    margin-top: 10px;
    text-align: center;
  }
  .read-btn {
    position: absolute;
    bottom: 0;
    left: 0;
    z-index: 1100;
    width: 100%;
    border-radius: 0 0 15px 15px;
    padding: 15px 0;
    text-align: center;
    color: white;
    font-size: 14px;
    background: rgb(74, 171, 255);
  }
</style>
