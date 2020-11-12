<template>
  <div class="w-tabs">
    <ul class="w-tabs-nav" ref="container">
      <li
        class="w-tabs-nav-item"
        :class="{selected: title === value}"
        :ref="el => { if (title=== value) selectedItem = el }"
        v-for="(title, index) in titles"
        :class="{selected: title === value}"
        :key="`${title}_${index}`"
        @click="select(title)"
      >{{title}}</li>
      <span class="w-tabs-nav-indicator" ref="indicator"></span>
    </ul>
    <main class="w-tabs-content">
      <component :is="current" :key="current.props.title" />
    </main>
  </div>
</template>

<script lang="ts">
import Tab from "./Tab.vue"
import { computed, onMounted, watchEffect, ref } from 'vue'
export default {
  name: "Tabs",
  components: {Tab},
  props: {
    value: {
      type: String,
    }
  },
  setup(props, context) {
    const selectedItem = ref < HTMLDivElement > (null)
    const indicator = ref < HTMLDivElement > (null)
    const container = ref < HTMLDivElement > (null)
    onMounted(() => {
      watchEffect(() => {
        const {
          width
        } = selectedItem.value.getBoundingClientRect()
        indicator.value.style.width = width + 'px'
        const {
          left: left1
        } = container.value.getBoundingClientRect()
        const {
          left: left2
        } = selectedItem.value.getBoundingClientRect()
        const left = left2 - left1
        indicator.value.style.left = left + 'px'
      }, {
        flush: 'post'
      })
    })
    const slots = context.slots.default()
    const titles = []
    slots.forEach((tag, i) => {
      if (tag.type.name !== Tab.name) {
        throw new Error('Tabs component need Tab!!!')
      }
      // 给一个默认的title
      const title = tag.props.title || `tab-title-${i}`
      titles.push(title)
    })
    // 如果不传value, 默认第一个
    if (!props.value && titles[0]) {
      context.emit('update:value', titles[0])
    }
    const current = computed(() => slots.find(tag => tag.props.title === props.value))
    const select = (title: string) => {
      context.emit('update:value', title)
    }
    return {
      current,
      titles,
      indicator,
      selectedItem,
      container,
      select
    }
  }
}
</script>

<style lang="scss">
$blue: #40a9ff;
$color: #333;
$border-color: #d9d9d9;
.w-tabs {
  &-nav {
    display: flex;
    list-style: none;
    color: $color;
    border-bottom: 1px solid $border-color;
    position: relative;
    &-item {
      padding: 8px 0;
      margin: 0 16px;
      cursor: pointer;
      /*&:first-child {
        margin-left: 0;
      }*/
      &.selected {
        color: $blue;
      }
    }
    &-indicator {
      position: absolute;
      height: 3px;
      background: $blue;
      left: 0;
      bottom: -1px;
      width: 100px;
      transition: all 250ms;
    }
  }
  &-content {
    padding: 8px 0;
  }
}
</style>