<template>
  <div ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: "Scroll",
    props: {
      probeType: {
        type: Number,
        default: 1
      },
      data: {
        type: Array,
        default: () => {
          return []
        }
      },
    },
    data() {
      return {
        scroll: null
      }
    },
    mounted() {
      this.scroll = new BScroll(this.$refs.wrapper, {
        probeType: this.probeType,
        click: true,
        pullUpLoad: this.pullUpLoad
      })

      this.scroll.on('scroll', (pos) => {
        this.$emit('scroll', pos)
      })
    },
    methods: {
      __initScroll() {
        // 1.初始化BScroll对象
        if (!this.$refs.wrapper) return


        // 2.将监听事件回调


        // 3.监听上拉到底部

      },
      refresh() {
        this.scroll && this.scroll.refresh()
      },
      finishPullUp() {
        this.scroll.finishPullUp()
      },
      scrollTo(x, y, time = 500) {
        this.scroll && this.scroll.scrollTo(x, y, time)
      }
    },
    watch: {
      data() {
        setTimeout(this.refresh, 20)
      }
    }
  }
</script>

<style scoped>

</style>
