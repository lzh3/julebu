<template>
	<view class="container">
		我的收藏
		<view class="list">
			<view class="item bg-white">
				<view class="pic"></view>
				<view class="title"></view>
				<view class="del">
					<image src="../../../static/image/icon/personal/del.png" mode=""></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				collectList:[
					{
						pic:'',
						title:'AMD出大招狙击英特尔'
					},
					{
						pic:'',
						title:'美国AMD半导体公司专门为计算机、通信和消费电子行业设计和制造各种创新的微处理器（CPU、GPU、主板芯片组、电视卡芯片等)，以及提供闪存和低功率处理器解决方案，公司成立于1969年。'
					},
					{
						pic:'',
						title:'AMD出大招狙击英特尔AMD出大招狙击英特尔'
					}
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
.item{
	position: relative;
	width:100%;
	height:210rpx;
	padding:30rpx;
	box-sizing: border-box;
	.pic{
		float:left;
		width:250rpx;
		height: 150rpx;
		background-color: skyblue;
		image{
			width:100%;
			height:100%;
		}
	}
	.title{
		float:left;
		word-wrap: wrap;
	}
	.del{
		position:absolute;
		right:30rpx;
		bottom:30rpx;
		width:16px;
		height:16px;
		image{
			width:100%;
			height:100%;
		}
	}
	
}
</style>
