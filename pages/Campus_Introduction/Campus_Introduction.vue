<template>
  <view class="content">
    <view class="uni-margin-wrap">
      <swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
        <swiper-item >
          <image src="../../static/new.jpg" mode="aspectFill" class="swiper-item"></image>
        </swiper-item>
        <swiper-item >
          <image src="../../static/old.jpg" mode="aspectFill" class="swiper-item"></image>
        </swiper-item>
      </swiper>
    </view>
    <view class="nav">
      <view class="logo">
        <image src="../../static/logo.webp" mode="aspectFill"></image>
        <text>九江职业技术学院</text>
      </view>
      <view class="tips">
        <text>中国特色高水平高职学校立项建设单位</text>
        <text>国家优质专科高等职业院校</text>
        <text>全国职业教育先进单位</text>
        <text>国家示范性高职院校</text>
        <text>建校时间：1960年</text>
        <text>办学类型：公办全日制</text>
        <text>所在城市：江西九江</text>
        <text>在校师生：2.2万余人</text>
      </view>
      <view class="grid">
        <cc-gonggeGrid gridNum="4" :gridList="gridList" @gridClick="gridClick"></cc-gonggeGrid>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        indicatorDots: true,
        autoplay: true,
        interval: 2000,
        duration: 500,
        gridList: [{
                        name: '校内导航',
                        imgSrc: "../../../../static/1.svg",
                    },
                    {
                        name: '学校官网',
                        imgSrc: "../../../../static/2.svg",
                    },
                    {
                        name: '校园官微',
                        imgSrc: "../../../../static/3.svg",
                    },
                    {
                        name: '招生咨询',
                        imgSrc: "../../../../static/4.svg",
                    }
                ]
      };
    },
    methods:{
      gridClick(item, index) { //格子菜单点击事件
        let list = ['/pages/index/index','https://www.jvtc.jx.cn/','QQ：2354130000','#小程序://九职招考天地/cmGYK8UjyMEOjbB']
        if(index == 0){
          uni.navigateBack()
        }else if(index == 3){
          wx.navigateToMiniProgram({
          	    shortLink:list[index],
          	    //develop开发版；trial体验版；release正式版
          	    envVersion: 'release', 
          	    success(res) {
          	      // 打开成功
          	      console.log("跳转小程序成功！",res);
          	    } 
          	})
        }else{
          uni.setClipboardData({
          	data: list[index],
          	success: function () {
              uni.showModal({
              	title: '提示',
              	content: '链接已复制，打开浏览器查看',
                showCancel:false,
              	success: function (res) {
              			console.log('用户点击确定');
              	}
              });
          	}
          });
        }
      }
    }
  }
</script>

<style lang="less" scoped>
.content{
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
.uni-margin-wrap {
		width: 100%;
		width: 100%;
    margin-top: 10px;
	}
	.swiper {
		height: 300rpx;
	}
	.swiper-item {
		display: block;
		height: 300rpx;
		margin: 0 auto;
    border-radius: 10px;
	}
  .nav{
    width: 100%;
    height: auto;
    position: relative;
  }
  .logo{
    width: 100%;
    height: 100px;
    position: absolute;
    top: -40px;
    image{
      display: block;
      width: 80px;
      height: 80px;
      border-radius:50% ;
      margin: 0 auto;
    }
    text{
      display: block;
      width: 100%;
      height: 20px;
      text-align: center;
    }
  }
  .tips{
    width: 100%;
    height: 100px;
    position: relative;
    top: 70px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-evenly;
    align-items: center;
    text{
      display: block;
      width: 50%;
      height: 20px;
      text-align: center;
      font-size: 12px;
      white-space: nowrap;  
      overflow: hidden;  
      text-overflow: ellipsis;
      font-weight: bold;
    }
  }
  .grid{
    margin-top: 50px;
  }
</style>
