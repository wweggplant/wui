<template>
  <button :class="{checked }" @click="toggle">
    <span></span>
  </button>
</template>

<script lang="ts">
import {ref} from 'vue'
export default {
  name: "Switch",
  setup() {
    let checked = ref(false);
    return {
      checked,
      toggle() {
        checked.value = !checked.value
      }
    }
  }
}
</script>
<style scoped lang="scss">
$h: 22px;
$h2:$h - 4px;
button{
  height: $h;
  width: $h*2;
  border: none;
  background: blue;
  border-radius: $h/2;
  position: relative;
}
span{
  position: absolute;
  top: 2px;
  left: 2px;
  height: $h2;
  width: $h2;
  background: white;
  border-radius: $h / 2;
}
button.checked{
  background: blue;
}
button.checked > span {
  left: calc(100% - #{$h2} - 2px)
}
button:focus{
  outline: none;
}
</style>