<template>
  <div ref="scroll">
    <slot></slot>
  </div>
</template>

<script>
import BSroll from 'better-scroll'
// import Vue from 'vue'
export default {
  props: {
    click: { //  点击事件
      type: Boolean,
      default: true
    },
    probeType: {
      type: Number,
      default: 1
    },
    /*
      当 probeType 为 1 的时候，会非实时（屏幕滑动超过一定时间后）派发scroll 事件；
      当 probeType 为 2 的时候，会在屏幕滑动的过程中实时的派发 scroll 事件；
      当 probeType 为 3 的时候，不仅在屏幕滑动的过程中，而且在 momentum 滚动动画运行过程中实时派发 scroll 事件。
      如果没有设置该值，其默认值为 0，即不派发 scroll 事件。
    */
    data: { //  传入data数据
      type: [Array, Object],
      default: null
    },
    listenScroll: { //  是否监听scroll
      type: Boolean,
      default: false
    },
    scrollTop: {
      type: Boolean,
      default: false
    }
  },
  mounted() {
    setTimeout(() => {
      this._initScroll()
    }, 20)
  },
  methods: {
    _initScroll() {
      if (!this.$refs.scroll) {
        return
      }
      this.scroll = new BSroll(this.$refs.scroll, {
        probeType: this.probeType,
        click: this.click
      })
      if (this.listenScroll) {
        const _this = this
        this.scroll.on('scroll', pos => {
          _this.$emit('scroll', pos)
        })
      }
    },
    enable() { //  启用better-scroll
      this.scroll && this.scroll.enable()
    },
    disable() { //  禁用better-scroll
      this.scroll && this.scroll.disable()
    },
    refresh() {
      // 刷新scroll 重新计算高度
      // 只有调用refresh才会滚动
      this.scroll && this.scroll.refresh()
    },
    scrollTo() { //  滚动到指定高度
      this.scroll && this.scroll.scrollTo.apply(this.scroll, arguments)
    },
    scrollToElement() { //  滚动到指定元素
      this.scroll && this.scroll.scrollToElement.apply(this.scroll, arguments)
    }
  },
  watch: {
    data() {
      setTimeout(() => {
        this.refresh()
        this.scroll.scrollTo(0, 0, 30)
      }, 20)
    },
    scrollTop() {
      // if (this.scrollTop) {
      // this.scroll.scrollTo(0,0,400)
      // }
    }
  }
}
</script>

<style lang="stylus">

</style>


