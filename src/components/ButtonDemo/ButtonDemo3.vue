<demo>
支持 type
</demo>
<template>
  <div>
    <div>
      <Button type="primary">主要按钮</Button>
      <Button>普通按钮</Button>
      <Button type="danger">危险按钮</Button>
    </div>
    <div>
      <Button theme="link" type="primary">主要链接按钮</Button>
      <Button theme="link">普通链接按钮</Button>
      <Button theme="link" type="danger">危险链接按钮</Button>
    </div>
    <div>
      <Button theme="text" type="primary">主要文字按钮</Button>
      <Button theme="text">普通文字按钮</Button>
      <Button theme="text" type="danger">危险文字按钮</Button>
    </div>
  </div>
</template>

<script lang="ts">
import {
  Button
} from "../../lib/index";
export default {
  components: {
    Button
  }
}
</script>