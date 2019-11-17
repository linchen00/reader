<template>
  <div class="ebook-bookmark" ref="bookmark">
    <div class="ebook-bookmark-text-wrapper">
      <div class="ebook-bookmark-down-wrapper" ref="iconDown">
        <span class="font_family">&#xe614;</span>
      </div>
      <div class="ebook-bookmark-text">{{text}}</div>
    </div>
    <div class="ebook-bookmark-icon-wrapper" :style="isFixed ? fixedStyle : {}">
      <bookmark :color="color" :width="15" :height="35"></bookmark>
    </div>
  </div>
</template>

<script>
  import Bookmark from "components/common/bookmark/Bookmark";
  import {ebookMixin} from "utils/mixin";
  import {getBookmark, saveBookmark} from "utils/localStorage";

  const BLUE = '#346cbc'
  const WHITE = '#fff'
  export default {
    name: "EbookBookmark",
    mixins: [ebookMixin],
    data() {
      return {
        text: '',
        color: '',
        isFixed: false
      }
    },
    components: {
      Bookmark
    },
    computed: {
      height() {
        return 35
      },
      threshold() {
        return 55
      },
      fixedStyle() {
        return {
          position: 'fixed',
          top: 0,
          right: `${(window.innerWidth - this.$refs.bookmark.clientWidth)}`
        }
      }
    },
    watch: {
      offsetY(v) {
        if (!this.menuVisible && this.bookAvailable) {
          if (v >= this.height && v <= this.threshold) {
            this.beforeThreshold(v)
          } else if (v > this.threshold) {
            this.afterThreshold(v)
          } else if (v > 0 && v < this.height) {
            this.beforeHight()
          } else if (v === 0) {
            this.restore()
          }
        }
      },
      isBookmark(isBookmark) {
        if (isBookmark) {
          this.color = BLUE
          this.isFixed = true
        } else {
          this.color=WHITE
          this.isFixed=false
        }
      }
    },
    methods: {
      addBookmark() {
        this.bookmark = getBookmark(this.fileName)
        if (!this.bookmark) {
          this.bookmark = []
        }

        const currentLocation = this.currentBook.rendition.currentLocation()
        const cfibase = currentLocation.start.cfi.replace(/!.*/, '')
        const cfistart = currentLocation.start.cfi.replace(/.*!/, '').replace(/\)$/, '')
        const cfiend = currentLocation.end.cfi.replace(/.*!/, '').replace(/\)$/, '')
        const cfirange = `${cfibase}!,${cfistart},${cfiend})`
        this.currentBook.getRange(cfirange).then(range => {
          const text = range.toString().replace(/\s\s/g, '')
          this.bookmark.push({
            cfi: currentLocation.start.cfi,
            text: text
          })
          saveBookmark(this.fileName, this.bookmark)
        })
      },
      removeBookmark() {
        const currentLocation = this.currentBook.rendition.currentLocation()
        const cfi = currentLocation.start.cfi
        this.bookmark = getBookmark(this.fileName)
        if (this.bookmark) {
          this.bookmark = this.bookmark.filter(item => item.cfi !== cfi)
          saveBookmark(this.fileName, this.bookmark)
          this.setIsBookmark(false)
        }
      },
      restore() {
        setTimeout(() => {
          this.$refs.bookmark.style.top = `${-this.height}px`
          this.$refs.iconDown.style.transform = 'rotate(0deg)'
        }, 200)
        if (this.isFixed) {
          this.setIsBookmark(true)
          this.addBookmark()
        } else {
          this.setIsBookmark(false)
          this.removeBookmark()
        }
      },
      beforeHight() {
        //状态一：未超过书签的高度
        if (this.isBookmark) {
          this.text = '下拉删除书签'
          this.color = BLUE
          this.isFixed = true
        } else {
          this.text = '下拉添加书签'
          this.color = WHITE
          this.isFixed = false
        }
      },
      beforeThreshold(v) {
        // 状态2：未到达零界状态
        this.$refs.bookmark.style.top = `${-v}px`
        if (this.isBookmark) {
          this.text = '下拉删除书签'
          this.color = BLUE
          this.isFixed = true
        } else {
          this.text = '下拉添加书签'
          this.color = WHITE
          this.isFixed = false
        }

        const iconDown = this.$refs.iconDown
        if (iconDown.style.transform === 'rotate(180deg)') {
          iconDown.style.transform = 'rotate(0deg)'
        }
      },
      afterThreshold(v) {
        // 状态3：超越零界状态
        this.$refs.bookmark.style.top = `${-v}px`
        if (this.isBookmark) {
          this.text = '松手删除书签'
          this.color = WHITE
          this.isFixed = false
        } else {
          this.text = '松手添加书签'
          this.color = BLUE
          this.isFixed = true
        }
        const iconDown = this.$refs.iconDown
        if (iconDown.style.transform === '' || iconDown.style.transform === 'rotate(0deg)') {
          iconDown.style.transform = 'rotate(180deg)'
        }
      },

    }
  }
</script>

<style scoped>
  .ebook-bookmark {
    position: absolute;
    top: -35px;
    left: 0;
    z-index: 200;
    width: 100%;
    height: 35px;
  }

  .ebook-bookmark-text-wrapper {
    position: absolute;
    right: 45px;
    bottom: 0;
    display: flex;
  }

  .ebook-bookmark-down-wrapper {

    color: #fff;
    transition: all 0.2s linear;
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }

  .ebook-bookmark-down-wrapper .font_family {
    font-size: 14px;
  }

  .ebook-bookmark-text {
    font-size: 14px;
    color: #fff;
  }

  .ebook-bookmark-icon-wrapper {
    position: absolute;
    right: 0;
    bottom: 0;
    margin-right: 15px;
  }

</style>
