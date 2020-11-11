<template>
  <Dialog v-model:visible="visible" >
    <template v-slot:content>
      <strong>hi</strong>
      <div>hi2</div>
    </template>
    <template v-slot:title>
      <strong>加粗的标题</strong>
    </template>
  </Dialog>
  <Button @click="visible = true">打开dialog</Button>

</template>

<script lang="ts">
import Dialog from '../lib/Dialog.vue'
import Button from '../lib/Button.vue'
import { ref } from 'vue'
export default {
  components: {
    Dialog,
    Button
  },
  setup() {
    let visible = ref(false)
    return {
      visible
    }
  }
}
</script>