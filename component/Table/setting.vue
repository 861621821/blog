<!--
 * @Descripttion:
 * @version:
 * @Author: lw
 * @Date: 2021-03-26 10:46:22
 * @LastEditTime: 2021-03-30 17:46:09
-->
<template>
  <div class="xl-ly-setting">
    <div class="handler-left">
      <slot/>
    </div>
    <div class="handler-right">
      <slot name="handlerright"/>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {

    }
  }
}
</script>
<style lang="scss" scoped>
.xl-ly-setting{
  display: flex;
  align-items: center;
  .handler-right{
    margin-left: auto;
  }
  .handler-left, .handler-right{
    margin-bottom: 24px;
    &:empty{
      margin-bottom: 0;
    }
  }
}
</style>
