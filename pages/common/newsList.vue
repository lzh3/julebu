<template>
	<view class="list">
		<view v-for="item in dataList">
			<navigator :url='item.url +"?title="+item.title' class="item bg-white">
				<view class="pic"></view>
				<view class="title">{{item.title}}</view>
				<view class="del">
					<image src="../../static/image/icon/personal/del.png" mode=""></image>
				</view>
			</navigator>
		</view>
	</view>
</template>

<script>
	export default {
		props:['dataList'],
		data() {
			return {
				
			}
		},
		onLoad(opt){
			console.lop(opt)
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.item {
		position: relative;
		width: 100%;
		height: 210rpx;
		margin-bottom: 10rpx;
		padding: 30rpx;
		box-sizing: border-box;

		.pic {
			float: left;
			width: 250rpx;
			height: 150rpx;
			background-color: skyblue;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.title {
			float: left;
			width: 414rpx;
			margin-left: 15rpx;
			overflow: hidden;
			text-overflow: ellipsis;
			display: -webkit-box;
			-webkit-line-clamp: 3;
			-webkit-box-orient: vertical;
		}

		.del {
			position: absolute;
			right: 30rpx;
			bottom: 30rpx;
			width: 16px;
			height: 16px;

			image {
				width: 100%;
				height: 100%;
			}
		}

	}
</style>
