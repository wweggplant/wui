<template>
  <Tabs v-model:value="activeName" >
    <Tab title="tab1">tab1的内容</Tab>
    <Tab title="tab2">tab2的内容</Tab>
  </Tabs>
</template>

<script lang="ts">
import Tabs from '../lib/Tabs/Tabs.vue'
import Tab from '../lib/Tabs/Tab.vue'
export default {
  components: {
    Tabs,
    Tab
  },
  setup() {
    return  {
      activeName: 'tab1'
    }
  }
}
</script>