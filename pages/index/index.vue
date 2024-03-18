<template>
	<view class="content">
		<image src="../../static/back.jpeg" mode="aspectFill"></image>
    <view class="center" :style="{top:statusBarHeight}">
      <text>九职校内导航</text>
    </view>
    <view class="footer" :style="{top:footerHeight}">
      <button @click="navigatorToCam">学校简介</button>
      <button @click="navigatorToCamNavigation">校内导航</button>
    </view>
	</view>
</template>

<script>
  export default {
    data() {
      return {
        statusBarHeight:0,
        footerHeight:0
      };
    },
    methods:{
      navigatorToCam(){
        uni.navigateTo({
          url:"/pages/Campus_Introduction/Campus_Introduction"
        })
      },
      navigatorToCamNavigation(){
        uni.navigateTo({
          url:"/pages/Campus_navigation/Campus_navigation"
        })
      }
    },
    onLoad() {
      let res = uni.getSystemInfoSync();
      let menu = wx.getMenuButtonBoundingClientRect();
      this.statusBarHeight = (menu.top - res.statusBarHeight) * 2 + menu.height + res.statusBarHeight + 10;
      if (res.model.indexOf('iPhone') > -1) {
        this.statusBarHeight += 4
      }
      this.footerHeight = this.statusBarHeight + 150 + 'px'
      this.statusBarHeight += "px"
    },
  }
</script>

<style lang="less" scoped>
	.content{
    width: 100vw;
    height: 100vh;
    position: relative;
    overflow: hidden;
    image{
      width: 100%;
      height: 100%;
    }
  }
  .center{
    position: absolute;
    width: 100%;
    height: 30px;
    text{
      display: block;
      width: 100%;
      height: 100%;
      text-align: center;
      font-family: '楷体';
      font-size: 32px;
      letter-spacing: 1rem;
      color: white;
    }
  }
  .footer{
    width: 100%;
    height: 40px;
    position: absolute;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;
    
  }
</style>
