<template>
	<view>
		<view class="grid-title">

			<text style="margin-left: 20px; color: #333333;">{{gridTitle}}</text>

		</view>
		<view class="grid">
			<view class="grid-item" :style="{width: 100/gridNum + '%'}" v-for="(item, index) in gridList" :key="index"
				@click.stop="gridClick(item, index)">
				<image :src="item.imgSrc" mode="aspectFill"></image>
				<view v-text="item.name"></view>
			</view>
		</view>

	</view>
</template>

<script>
	export default {

		props: {
			// 一行展示几个格子
			gridNum: {
				type: [String, Number],
				default: 3
			},
			// 左边标题栏
			gridTitle: {
				type: String,
				default: ""
			},
			// 宫格数组
			gridList: {
				type: Array,
				default: () => {
					return [];
				}
			}


		},
		data() {
			return {

			}
		},
		methods: {
			gridClick(item, index) {


				this.$emit("gridClick", item, index);

			},

		}

	}
</script>

<style lang="less" scoped>
	.grid-title {
		display: flex;
		align-items: center;
		font-size: 32upx;
		color: rgba(0, 0, 0, .63);
		padding: 30upx 0;


	}

	.grid {
		display: flex;
		align-items: center;
		flex-wrap: wrap;
		border-top: 1upx solid rgba(172, 172, 172, .2);

		.grid-item {
			box-sizing: border-box;
			// 设置是九宫格还是十二宫格 /3九宫格 /4十二宫格
			width: calc(100% / 3);
			border-bottom: 1upx solid rgba(172, 172, 172, .2);
			border-right: 1upx solid rgba(172, 172, 172, .2);
			text-align: center;
			padding: 40upx 0;
			position: relative;

			// 图片尺寸
			image {

				width: 54upx;
				height: 54upx;
				align-items: center;
			}

			view {
				font-size: 28upx;
				margin-top: 4upx;
				color: #666666;
			}

		}


	}
</style>