<template>
  <div class="tab">
    <cube-tab-bar
      :useTransition=false
      :showSlider=true
      v-model="selectedLabel"
      :data="tabs"
      ref="tabBar"
      class="border-bottom-1px">
    </cube-tab-bar>
    <div class="slide-wrapper">
      <cube-slide
        :loop=false
        :auto-play=false
        :show-dots=false
        :initial-index="index"
        ref="slide"
        @change="onChange"
        @scroll="onScroll"
        :options="slideOptions"
        >
        <cube-slide-item v-for="(tab,index) in tabs" :key="index">
          <component :is="tab.component" :data="tab.data"></component>
        </cube-slide-item>
      </cube-slide>
    </div>
  </div>
</template>

<script>

export default {
  name: 'tab',
  props: {
    tabs: {
      type: Array,
      default() {
        return {}
      }
    }
  },
  data() {
    return {
      index: 0,
      slideOptions: {
        listenScroll: true,
        probeType: 3,
        derectionLockThreshold: 0
      }
    }
  },
  computed: {
    selectedLabel: {
      get() {
        return this.tabs[this.index].label
      },
      set(newVal) {
        this.index = this.tabs.findIndex((value) => {
          return value.label === newVal
        })
      }
    }
  },
  methods: {
    onChange(current) {
      this.index = current
    },
    onScroll(pos) {
      const tabBarWidth = this.$refs.tabBar.$el.clientWidth
      const slideWidth = this.$refs.slide.slide.scrollerWidth
      const transform = -pos.x / slideWidth * tabBarWidth
      console.log(transform)
      this.$refs.tabBar.setSliderTransform(transform)
    }
  }
}
</script>

<style lang="stylus" scoped>
@import "~common/stylus/variable"

.tab
  >>> .cube-tab
    padding: 10px 0
  display: flex
  flex-direction: column
  height: 100%
  .slide-wrapper
    flex: 1
    overflow: hidden
</style>
