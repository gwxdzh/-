# cc-gonggeGrid


#### 使用方法 
```使用方法
	
<!-- gridTitle:宫格名称 gridNum: 一行展示格数 gridList：宫格数据 @gridClick:宫格点击按钮 -->
<cc-gonggeGrid gridTitle="九宫格" gridNum="3" :gridList="gridList" @gridClick="gridClick"></cc-gonggeGrid>

<!-- gridTitle:宫格名称 gridNum: 一行展示格数 gridList：宫格数据 @gridClick:宫格点击按钮 -->
<cc-gonggeGrid gridTitle="十二宫格" gridNum="4" :gridList="gridList" @gridClick="gridClick"></cc-gonggeGrid>

<!-- gridTitle:宫格名称 gridNum: 一行展示格数 gridList：宫格数据 @gridClick:宫格点击按钮 -->
<cc-gonggeGrid gridTitle="十五宫格" gridNum="5" :gridList="gridList" @gridClick="gridClick"></cc-gonggeGrid>


```

#### HTML代码实现部分
```html
<template>
	<view>

		<!-- gridTitle:宫格名称 gridNum: 一行展示格数 gridList：宫格数据 @gridClick:宫格点击按钮 -->
		<cc-gonggeGrid gridTitle="九宫格" gridNum="3" :gridList="gridList" @gridClick="gridClick"></cc-gonggeGrid>

		<!-- gridTitle:宫格名称 gridNum: 一行展示格数 gridList：宫格数据 @gridClick:宫格点击按钮 -->
		<cc-gonggeGrid gridTitle="十二宫格" gridNum="4" :gridList="gridList" @gridClick="gridClick"></cc-gonggeGrid>

		<!-- gridTitle:宫格名称 gridNum: 一行展示格数 gridList：宫格数据 @gridClick:宫格点击按钮 -->
		<cc-gonggeGrid gridTitle="十五宫格" gridNum="5" :gridList="gridList" @gridClick="gridClick"></cc-gonggeGrid>

	</view>
</template>

<script>
	export default {
		components: {

		},
		data() {
			return {

				gridList: [{
						name: '功能1',
						imgSrc: "../../static/appointList.svg",
					},
					{
						name: '功能2',
						imgSrc: "../../static/appointNum.svg",
					},
					{
						name: '功能3',
						imgSrc: "../../static/appointList.svg",
					},
					{
						name: '功能4',
						imgSrc: "../../static/appointNum.svg",
					},
					{
						name: '功能5',
						imgSrc: "../../static/appointList.svg",
					},
					{
						name: '功能6',
						imgSrc: "../../static/appointNum.svg",
					},
					{
						name: '功能7',
						imgSrc: "../../static/appointList.svg",
					},
					{
						name: '功能8',
						imgSrc: "../../static/appointNum.svg",
					},
					{
						name: '功能9',
						imgSrc: "../../static/appointNum.svg",
					},


				]
			}
		},

		methods: {
			gridClick(item, index) { //格子菜单点击事件



				console.log('item = ' + item.name + 'index = ' + index);
				uni.showModal({
					title: '温馨提示',
					content: '点击的功能是: ' + item.name
				})


			}
		}
	}
</script>

<style lang="less" scoped>

</style>
```
