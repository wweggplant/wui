<template>
<div class="w-button" :class="classes">
  <span v-if="loading" class="w-loadingIndicator"></span>
  <slot />
</div>
</template>

<script lang="ts">
import { computed } from 'vue'
declare const props: {
  theme?: 'button' | 'text' | 'link';
  size?: 'normal' | 'big' | 'small';
  type?: 'normal' | 'primary' | 'danger';
  disabled: boolean;
  loading: boolean;
}
export default {
  name: "Button",
  unheritAttrs: false,
  props: {
    theme: {
      type: String,
      default: "button",
    },
    size: {
      type: String,
      default: "normal",
    },
    type: {
      type: String,
      default: "normal",
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    loading: {
      type: Boolean,
      default: false
    }
  },
  setup(props, context) {
    const classes = computed(() => [
      `w-theme-${props.theme}`,
      `w-size-${props.size}`,
      `w-type w-type-${props.type} w-type-${props.theme}-${props.type}`,
      {'w-disabled': props.disabled}
    ])
    return {
      classes
    }
  }
}
</script>

<style lang="scss">
$h: 32px;
$border-color: #d9d9d9;
$color: #333;
$blue: #40a9ff;
$radius: 4px;
$red: red;
$grey: grey;
.w-button {
  box-sizing: border-box;
  height: $h;
  padding: 0 12px;
  cursor: pointer;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  white-space: nowrap;
  background: white;
  color: $color;
  border: 1px solid $border-color;
  border-radius: $radius;
  box-shadow: 0 1px 0 fade-out(black, 0.95);
  transition: background 250ms;
  & + & {
    margin-left: 8px;
  }
  &:hover,
  &:focus {
    color: $blue;
    border-color: $blue;
  }
  &:focus {
    outline: none;
  }
  &::-moz-focus-inner {
    border: 0;
  }
  &.w-theme-link,
  &.w-theme-text{
    border-color: transparent;
    box-shadow: none;
    background: 0 0;
  }
  &.w-theme-link {
    color: $blue;
    &:hover,
    &:focus {
      color: lighten($blue, 10%);
      background: rgba(0,0,0,.018);
    }
  }
  &.w-theme-text {
    color: inherit;
    &:hover,
    &:focus {
      background: rgba(0,0,0,.018);
    }
  }
  &.w-size-big {
    font-size: 24px;
    height: 48px;
    padding: 0 16px;
  }
  &.w-size-small {
    font-size: 12px;
    height: 20px;
    padding: 0 4px;
  }
  &.w-type {
    &.w-type-primary {
      background: $blue;
      color: white;
      border-color: $blue;
      &:hover,
      &:focus {
        background: darken($blue, 10%);
        border-color: darken($blue, 10%);
      }
    }
    &.w-type-danger {
      background: $red;
      border-color: $red;
      color: white;
      &:hover,
      &:focus {
        background: darken($red, 10%);
        border-color: darken($red, 10%);
      }
    }
  }
  &.w-type-link {
    &.w-type-link-danger {
      color: $red;
      &:hover,
      &:focus {
        color: darken($red, 10%);
      }
    }
  }
  &.w-type-text {
    &.w-type-text-primary {
      color: $blue;
      &:hover,
      &:focus {
        color: darken($blue, 10%);
      }
    }
    &.w-type-text-danger {
      color: $red;
      &:hover,
      &:focus {
        color: darken($red, 10%);
      }
    }
  }
  &.w-type {
    &.w-disabled {
      cursor: not-allowed;
      color: $grey;
      &:hover {
        border-color: $grey;
      }
    }
  }
  &.w-type-link, &.w-type-text {
    &.w-disabled {
      cursor: not-allowed;
      color: $grey;
    }
  }
  > .w-loadingIndicator{
    width: 14px;
    height: 14px;
    display: inline-block;
    margin-right: 4px;
    border-radius: 8px;
    border-color: $blue $blue $blue transparent;
    border-style: solid;
    border-width: 2px;
    animation: w-spin 1s infinite linear;
  }
}
@keyframes w-spin {
  0%{transform: rotate(0deg)}
  100%{transform: rotate(360deg)}
}
</style>