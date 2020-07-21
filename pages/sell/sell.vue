<template>
	<view class="bg-white">
		<view class="list">
			<view v-for="item in list" class="item">
				<navigator :url='item.url'>
					<view class="img">
						<image :src="item.pic" mode=""></image>
					</view>
					<view class="txt">{{item.detail}}</view>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
						detail: 'CPU销售',
						url: './jifenrule/jifenrule',
						pic:'../../static/image/sell/1.png'
					},
					{
						detail: '显卡销售',
						url: './jifenrule/jifenrule',
						pic:'../../static/image/sell/2.png'
					},
					{
						detail: '主板销售',
						url: './jifenrule/jifenrule',
						pic:'../../static/image/sell/3.png'
					},
					{
						detail: '优选销售',
						url: './jifenrule/jifenrule',
						pic:'../../static/image/sell/4.png'
					},
					{
						detail: '积分规则',
						//url: '../register/register',
						url:'../mustlook/mustlook',
						pic:'../../static/image/sell/5.png'
					},
				]
			}
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.list {
		width: 750rpx;
		margin: 0 auto;
		padding: 0 15rpx;
		display: flex;
		flex-wrap: wrap;

		.item {
			width: 180rpx;
			height: 180rpx;
			margin: 20rpx 30rpx;
			text-align: center;

			.img {
				width: 110rpx;
				height: 110rpx;
				margin: 0 auto;
				image{
					width:100%;
					height:100%;
				}
			}
			.txt{
				margin-top:5px;
			}
		}
	}
</style>
