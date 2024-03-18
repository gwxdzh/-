<template>
  <view class="content">
    <map class="order-map" latitude="29.670726" scale="16" longitude="116.014714" :enable-traffic="true" :show-compass="true" show-location :key="markers.length + new Date().getTime()" :markers="markers" @callouttap="callout">
    </map>
    <view class="order-top">
      <input type="search" placeholder="搜索地址" @tap="navigatorTosearch" v-model="searchValue"/>
    </view>
    <view class="order-box" v-if="endPoint.name">
                <view class="search-start">
                    <view class="custom-style" v-for="item in btnList" :key="item.value"
                        :class="{active:flag === item.value}" @click="selectRouteType(item.value)">
                        {{item.name}}
                    </view>
                </view>
                <view class="search-start" v-if="distance || duration">
                    <view class="start-name">
                        距离：{{distance}} 时间：{{duration}}
                    </view>
                </view>
                <view class="search-start" >
                    <view class="start-name">
                        您将在 <text style="color: #33a63b">{{Startaddress.name}}</text> 出发<br/>
                        前往<text style="color: #ffdc2b">{{endPoint.name}}</text>
                    </view>
                </view>
                <button @click="navigatorTotxdt" >开始出发</button>
            </view>
  </view>
</template>

<script>
  import amapFile from "../../libs/amap-wx.130.js";
  import {schoolLocation} from "../../libs/location.js"
  export default {
    data() {
      return {
        myAmapFunT:null,
        markers: [],
        searchValue:'',
        Startaddress:{
          longitude:0,
          latitude:0,
          name:'',
          address:''
        },
        endPoint: {},
        maxLocation:{
          maxlatitude:'29.676211917885936',
          maxlongitude:'116.01814186312103',
          minlongitude:'116.0108784411087',
          minlatitude:'29.66431217705956'
        },
        distance: 0, //距离
        duration: 0, //时间
        flag: 10,
        btnList: [{
                name: '推荐',
                value: 10
            },
            {
                name: '躲避拥堵',
                value: 4
            },
            {
                name: '距离短',
                value: 2
            },
        ]
      }
    },
    methods: {
      navigatorTosearch(){
        this.searchValue = ''
        uni.navigateTo({
          url:"/pages/search/search"
        })
      },
      navigatorTotxdt(){
        let endPoint = JSON.stringify({//终点
                'name':this.endPoint.name,
                'location':{
                    'lng':this.endPoint.longitude,
                    'lat':this.endPoint.latitude
                    }
            })
            let startPoint = JSON.stringify({//终点
                    'name':this.Startaddress.name,
                    'location':{
                        'lng':this.Startaddress.longitude,
                        'lat':this.Startaddress.latitude
                        }
                })
            uni.navigateToMiniProgram({
              appId:'wx7643d5f831302ab0',//要打开的小程序 appId
              path:'pages/multiScheme/multiScheme?endLoc='+endPoint+"&startLoc="+startPoint,//打开的页面路径，如果为空则打开首页
                fail:function(){
                    wx.showToast({
                      icon:'none',
                      title:'打开失败，请重试'
                    })
                }
              })
      },
      callout(event){
        this.clearIcon()
        let markerId = event.detail.markerId - 1
        if(markerId != 0){
          this.markers[markerId].iconPath = '../../static/start.png'
          this.endPoint = {
                            longitude: this.markers[markerId].longitude,
                            latitude: this.markers[markerId].latitude,
                            name: '九江职业技术学院(濂溪校区)'+this.markers[markerId].callout.content
                        }
          let start = this.Startaddress.longitude + ',' + this.Startaddress.latitude
          let end = this.endPoint.longitude + ',' + this.endPoint.latitude
          //每次选取位置完成后都会默认 10 策略
          this.flag = 10
          //生成规划路线
          this.getPlanningRoute(start, end, 10)
        }
      },
      clearIcon(){
        this.markers.forEach((item,index)=>{
          if(index != 0){
            item.iconPath = '../../static/biaoji.png'
          }else{
            item.iconPath = '../../static/end.png'
          }
        })
      },
      //获取当前位置
      getLocation(){
        uni.getLocation({
        	type: 'gcj02',
        	success: (res)=> {
            if(res.longitude > this.maxLocation.maxlongitude || res.longitude < this.maxLocation.minlongitude || res.latitude > this.maxLocation.maxlatitude || res.latitude < this.maxLocation.minlatitude){
              this.Startaddress.longitude = '116.01293837763214'
              this.Startaddress.latitude ='29.66535165433053'
              
            }else{
              this.Startaddress.longitude = res.longitude;
              this.Startaddress.latitude = res.latitude;
            }
            this.getAddress(this.Startaddress.longitude + ',' + this.Startaddress.latitude)
        	}
        });
      },
      // 转换成对应地址
      getAddress(loc) {
        var myAmapFun = this.myAmapFunT
        var that = this;
        if (loc !== null && loc !== '' && loc !== undefined) {
            myAmapFun.getRegeo({
                iconPath: '',
                width: '37.5rpx',
                height: '37.5rpx',
                location: loc,
                success: function(data) { //成功回调
                    that.Startaddress.name = that.Startaddress.longitude == '116.01293837763214' ? '新校区正大门' : '当前位置'
                    that.Startaddress.address = data[0].desc
                    that.initMap()
                },
                fail: function(info) { //失败回调
                    console.log(info)
                },
            });
        }
      },
      // 初始化地图
      //初始化地图数据
      initMap() {
        let startName = this.Startaddress.longitude == '116.01293837763214' ? '新校区正大门' : '当前位置'
        let SchoolLocation = schoolLocation
         SchoolLocation.unshift({
          longitude:this.Startaddress.longitude,
          latitude:this.Startaddress.latitude,
          name:startName
        })
        SchoolLocation.forEach((item,index)=>{
          this.markers.push({
              id: index + 1,
              latitude: item.latitude, //纬度
              longitude: item.longitude, //经度
              iconPath: index == 0 ? '../../static/end.png' : '../../static/biaoji.png', //显示的图标
              rotate: 0, // 旋转度数
              width: 30, //宽
              height: 30, //高
              callout:{
                content:item.name,
                bgColor:'#fff',
                borderRadius:10,
                padding:10,
                display:'BECLICK',
                textAlign:'center'
              },
          }, )
        })
      },
      selectRouteType(idx) {
          this.flag = idx
          let start = this.Startaddress.longitude + ',' + this.Startaddress.latitude
          let end = this.endPoint.longitude + ',' + this.endPoint.latitude
          this.getPlanningRoute(start, end, idx)
      },
      getPlanningRoute(start, end, strategy = 10) {
        let that = this
        uni.showLoading({
            title: '加载中'
        });
        that.myAmapFunT.getDrivingRoute({
            origin: start,
            destination: end,
            strategy: strategy, //备选方案
            success: function(data) {
                // console.log('所有路径',data)
                if (data.paths && data.paths[0] && data.paths[0].steps) {
                    // 默认 10 会 对返回多条路径的方案  按照时间短的
                    let goodRouter = data.paths.sort((a, b) => {
                        return a.duration - b.duration
                    })[0]

                    that.distance = (goodRouter.distance * 0.001).toFixed(2) + '公里'
                    that.duration = '大约' + (goodRouter.duration / 60).toFixed(2) + '分钟'

                    let steps = goodRouter.steps
                    let points = []
                    for (var i = 0; i < steps.length; i++) {
                        var poLen = steps[i].polyline.split(';');
                        for (var j = 0; j < poLen.length; j++) {
                            points.push({
                                longitude: parseFloat(poLen[j].split(',')[0]),
                                latitude: parseFloat(poLen[j].split(',')[1])
                            })
                        }
                    }
                    that.polyline = [{
                        points: points,
                        color: strategy === 10 ? '#0ee532' : strategy === 2 ? '#0742d9' :
                            '#ee6b06',
                        width: 8,
                    }]
                }
                uni.hideLoading();
            },
            fail: function(info) { //失败回调
                console.log('路线规划失败')
                console.log(info)
                uni.hideLoading();
                uni.showToast({
                    title: '路线规划失败',
                    icon: 'error'
                });
            },
        })
    },
    },
    onLoad() {
     this.myAmapFunT = new amapFile.AMapWX({
        key: 'f69a9ba7fc68561f36357f2c17f5fdac'
     })
     this.getLocation()
    },
    onShow() {
      let that = this;
      uni.$once('idx',function(data){
        that.clearIcon()
        
        let markerId = 0
        that.markers.filter((item,idx)=>{
          if(item.callout.content == data){
            markerId = idx
          }
        })
        if(markerId != 0){
          that.markers[markerId].iconPath = '../../static/start.png'
          that.endPoint = {
                            longitude: that.markers[markerId].longitude,
                            latitude: that.markers[markerId].latitude,
                            name: '九江职业技术学院(濂溪校区)'+that.markers[markerId].callout.content
                        }
          let start = that.Startaddress.longitude + ',' + that.Startaddress.latitude
          let end = that.endPoint.longitude + ',' + that.endPoint.latitude
          that.searchValue = that.markers[markerId].callout.content
          //每次选取位置完成后都会默认 10 策略
          that.flag = 10
          //生成规划路线
          that.getPlanningRoute(start, end, 10)
      }
    })
  }
  }
</script>

<style lang="less" scoped>
.content{
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
.order-map{
  width: 100%;
  height: 100%;
}
.newSchool{
  position: absolute;
  top: 17px;
  width: 110%;
  height: 112%;
  left: -25px;
}
.order-box {
        position: fixed;
        bottom: 0rpx;
        left: 0rpx;
        width: 100%;
        //height: 435rpx;
        text-align: left;
        background-color: #FFFFFF;
        border-top-right-radius: 10rpx;
        border-top-left-radius: 10rpx;
        box-sizing: border-box;
        padding: 18rpx;
        padding-bottom: 80rpx;
        box-shadow: 0 9.375rpx 28.125rpx 9.375rpx rgba(106, 66, 0, 0.2);

        .send-btn {
            margin-top: 30rpx;
            width: 100%;
            color: white;
            background-color: #ffa602;
            padding: 0 24rpx;
            font-size: 28rpx;
            height: 80rpx;
            line-height: 80rpx;
            box-sizing: border-box;
            border-radius: 12rpx;
            text-align: center;
        }

        /*box-shadow: 0 9.375rpx 28.125rpx 9.375rpx rgba(106, 66, 0, 0.2);*/
        .search-start {
            font-size: 30rpx;
            margin: 18rpx 0;
            padding-left: 15rpx;
            display: flex;
            justify-content: flex-start;
            align-items: center;
            box-sizing: border-box;

            .start-name {
                width: 550rpx;
                padding-left: 10rpx;
                overflow: hidden;
                /*文本不会换行*/
                white-space: nowrap;
                /*当文本溢出包含元素时，以省略号表示超出的文本*/
                text-overflow: ellipsis;
            }

            .custom-style {
                margin-right: 10rpx;
                border-radius: 6rpx;
                border: 1rpx solid #ebedf0;
                font-size: 24rpx;
                padding: 8rpx 24rpx;

                &:last-child {
                    margin-right: 0;
                }

                &.active {
                    color: #FFFFFF;
                    background-color: #ffa602;
                    border-color: #ffa602;
                }
            }
        }

        .search-box {
            background-color: #f3f3f3;
            border-radius: 36rpx;
            height: 80rpx;
            display: flex;
            justify-content: flex-start;
            align-items: center;
            box-sizing: border-box;
            padding: 0 25rpx;

            .search-placeholder {
                font-size: 28rpx;
                color: #bbb9b9;
                padding-left: 15rpx;
            }
        }
    }

    .addH {
        height: 420rpx;
        /*+100*/
    }

    .row {
        display: flex;
        flex-direction: row;
    }

    .bot {
        margin-bottom: 10rpx;
    }

    .license {
        position: relative;
        left: 30rpx;
        height: 110rpx;
        font-weight: bold;
        font-size: 38rpx;
        line-height: 130rpx;
        letter-spacing: 1.125rpx;
        /* border: 0.01rem solid #555555; */
    }

    .time-icon {
        height: 16rpx;
        width: 16rpx;
        position: relative;
        left: 190rpx;
        top: 59rpx;
    }

    .time-text {
        height: 110rpx;
        line-height: 130rpx;
        position: relative;
        left: 200rpx;
        font-size: 30rpx;
        color: #666666;
        /* border: 0.01rem solid #555555; */
    }

    .route-icon {
        height: 12rpx;
        width: 12rpx;
        position: relative;
        left: 42rpx;
        top: 30rpx;
    }

    .route-text {
        height: 65rpx;
        width: 478rpx;
        line-height: 65rpx;
        position: relative;
        left: 50rpx;
        font-size: 30rpx;
        color: #666666;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        /* border: 0.01rem solid #555555; */
    }

    .amt-box {
        width: calc(100% - 558rpx);
        margin-left: 40rpx;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        align-items: center;
        /* border: 1rpx solid #555555; */
    }

    .amt-icon {
        margin-top: 5rpx;
        line-height: 65rpx;
        height: 20rpx;
        width: 20rpx;
    }

    .amt {
        margin-left: 10rpx;
        line-height: 65rpx;

        /*line-height: 80rpx;*/
        font-size: 30rpx;
        color: #666666;
    }

    .todo {
        position: relative;
        height: 165rpx;
        width: 640rpx;
        margin-left: 30rpx;
        border-top: 0.375rpx solid #E9EAEC;
    }

    .todo-item {
        height: 100%;
        width: 50%;
        text-align: center;
        align-content: center;
        /* border: 0.01rem solid #555555; */
    }

    .todo-item>image {
        height: 60rpx;
        width: 60rpx;
        margin-top: 30rpx;
    }

    .todo-item>view {
        color: #3F3F3F;
        font-size: 30rpx;
        letter-spacing: 0.375rpx;
        /* border: 0.01rem solid #555555; */
    }
    .order-top{
      width: 100%;
      height: 35px;
      position: fixed;
      top: 0;
      padding: 5px 0;
      background-color: transparent;
      input{
        display: block;
        width: 90%;
        height: 95%;
        margin: 0 auto;
        margin-top: 0.875px;
        border: 1px solid #fff;
        border-radius: 14px;
        background-color: #fff;
        font-size: 12px;
        padding-left: 5px;
      }
    }
</style>
