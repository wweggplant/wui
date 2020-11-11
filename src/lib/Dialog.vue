<template>
<Teleport :to="appendToBody">
  <div class="w-dialog-root" :style="styles">
    <div class="w-dialog-mask" @click="onClickMask"></div>
    <div class="w-dialog-wrapper">
      <div class="w-dialog">
        <header>
          <slot name="title" />
          <span @click="close" class="w-dialog-close"></span>
        </header>
        <main>
          <slot name="content" />
        </main>
        <footer>
          <Button type="primary" @click="handleOk">{{okText}}</Button>
          <Button @click="handleCancel">{{cancelText}}</Button>
        </footer>
      </div>
    </div>
  </div>
</Teleport>
</template>

<script lang="ts">
import {ref, computed} from 'vue'
export default {
  name: "Dialog",
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    appendToBody: {
      type: String,
      default: 'body'
    },
    closeOnClickMask: {
      type: Boolean,
      default: true
    },
    okText: {
      type: String,
      default: '确定'
    },
    cancelText: {
      type: String,
      default: '取消'
    },
    cancel: {
      type: Function,
      default() {}
    },
    ok: {
      type: Function,
      default() {}
    }
  },
  setup(props, context) {
    let styles = computed(() => {
      if (!props.visible) {
        return {display: 'none'}
      } else {
        return ''
      }
    })
    let close = () => {
      context.emit('update:visible', false)
    }
    return  {
      styles,
      onClickMask() {
        this.closeOnClickMask && close()
      },
      close,
      // TODO cancel/ok的函数可能是异步的, 那是不是把close作为参数传到cancel/ok里更好呢?
      handleCancel() {
        props.cancel?.()
        close()
      },
      handleOk() {
        props.ok?.()
        close()
      }
    }
  }
}
</script>

<style lang="scss">
$radius: 4px;
$border-color: #d9d9d9;
.w-dialog {
  background: white;
  border-radius: $radius;
  box-shadow: 0 0 3px fade_out(black, 0.5);
  min-width: 15em;
  max-width: 90%;
  &-mask {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: fade_out(black, 0.5);
    z-index: 10;
  }
  &-wrapper {
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    z-index: 11;
  }
  >header {
    padding: 12px 16px;
    border-bottom: 1px solid $border-color;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 20px;
  }
  >main {
    padding: 12px 16px;
  }
  >footer {
    border-top: 1px solid $border-color;
    padding: 12px 16px;
    text-align: right;
  }
  &-close {
    position: relative;
    display: inline-block;
    width: 16px;
    height: 16px;
    cursor: pointer;
    &::before,
    &::after {
      content: '';
      position: absolute;
      height: 1px;
      background: black;
      width: 100%;
      top: 50%;
      left: 50%;
    }
    &::before {
      transform: translate(-50%, -50%) rotate(-45deg);
    }
    &::after {
      transform: translate(-50%, -50%) rotate(45deg);
    }
  }
}
</style>