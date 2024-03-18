<template>
  <view>
    
  </view>
</template>

<script>
  export default {
    data() {
      return {
        statusBarHeight:0,
      };
    },
    onLoad() {
      let res = uni.getSystemInfoSync();
      let menu = wx.getMenuButtonBoundingClientRect();
      this.statusBarHeight = (menu.top - res.statusBarHeight) * 2 + menu.height + res.statusBarHeight;
      if (res.model.indexOf('iPhone') > -1) {
        this.statusBarHeight += 4
      }
    },
  }
</script>

<style lang="less">

</style>
