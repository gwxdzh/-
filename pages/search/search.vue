<template>
  <view class="content">
    <view class="search-box">
      <input type="search" placeholder="请输入搜索地址" v-model="value"/>
      <text @tap="searchLocation">搜索</text>
    </view>
    <scroll-view class="search-list" :scroll-y="true">
      <view class="list-item" v-for="(item,idx) in searchList" :key="ids" @tap="goBack(idx)">
        <image :src="item.url" mode="aspectFill"></image>
        <text class="title">{{ item.name }}</text>
        <text class="jt">></text>
      </view>
      <text style="text-align: center;width: 100%;" v-show="searchList.length == 0">暂无该地址</text>
    </scroll-view>
  </view>
</template>

<script>
  import {schoolLocation} from '../../libs/location.js'
  export default {
    data() {
      return {
        value:'',
        searchList:[]
      };
    },
    methods:{
      goBack(id){
        let itemname = this.searchList[id].name
        uni.$emit("idx",itemname)
        uni.navigateBack()
      },
      searchLocation(){
        this.searchList = []
        this.searchList = schoolLocation.filter(item=>{
          return item.name.includes(this.value)
        })
      }
    },
    onLoad() {
      for(let i=0;i<8;i++){
        this.searchList.push(schoolLocation[i])
      }
    }
  }
</script>

<style lang="less">
.content{
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background-color: #E6E6E6;
}
.search-box{
  width: 100%;
  height: 35px;
  position: relative;
  input{
    display: block;
    width: calc(90% - 60px);
    height: 95%;
    border: 1px solid #fff;
    border-radius: 14px 0 0 14px;
    background-color: #fff;
    font-size: 12px;
    padding-left: 5px;
    margin: 3px 0 0 5%;
  }
  text{
    text-align: center;
    display: block;
    width: 60px;
    height: 24px;
    position: absolute;
    right: 2.5%;
    top: 0;
    font-size: 12px;
    border-radius: 0 14px 14px 0;
    color: #fff;
    background-color: #89c6ff;
    padding-top: 2.5%;
  }
}
.search-list{
  width: 100%;
  height: calc(100% - 45px);
  margin-top: 5px;
}
.list-item{
  width: 100%;
  height: 60px;
  background-color: #fff;
  position: relative;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  margin-bottom: 5px;
  image{
    display: block;
    width: 50px;
    height: 50px;
    margin-left: 5px;
  }
  .title{
    width: 100px;
    height: 20px;
    margin-left: 10px;
    font-size: 13px;
  }
  .jt{
    width: 20px;
    height: 20px;
    position: absolute;
    right: 10px;
    font-size: 20px;
  }
}
</style>
