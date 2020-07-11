<template>
	<view class="container">
		<view class="list">
			<view class="item">
				<view class="title">AMD yes最早起源于ryzen发布后老莱的视频，经过AMD吧和b站
弹幕发扬光大能不能战未来？我觉得OK</view>
				<view class="pic-list">
					<view class="pic"></view>
				</view>
				<view>
					<view class="text-gray text-sm text-left padding">
						<text class="cuIcon-time margin-lr-xs"></text> 2
						<text class="cuIcon-attention margin-lr-xs icon"></text> 2
						<text class="cuIcon-like margin-lr-xs icon"></text> 2
						<text class="cuIcon-appreciate margin-lr-xs icon"></text> 2
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataList:[
					{
						title:'',
						pic:[1,2,4],
						time:'gg',
						look:22,
						comment:20,
						dz:20
					}
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
	.list{
		.item{
			width:100%;
			height:333rpx;
			margin-top:5px;
			padding:30rpx;
			background-color: #fff;
		}
	}
</style>
