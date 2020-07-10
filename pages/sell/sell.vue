<template>
	<view>
		<view class="list">
			<view v-for="item in list" class="item">
				<navigator :url='item.url'>
					<view class="img"></view>
					<view>{{item.detail}}</view>
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
						url: './jifenrule/jifenrule'
					},
					{
						detail: 'CPU销售',
						url: './jifenrule/jifenrule'
					},
					{
						detail: 'CPU销售',
						url: './jifenrule/jifenrule'
					},
					{
						detail: 'CPU销售',
						url: './jifenrule/jifenrule'
					},
					{
						detail: 'CPU销售',
						url: './jifenrule/jifenrule'
					},
					{
						detail: '积分规则',
						url: './jifenrule/jifenrule'
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
			background-color: skyblue;

			.img {
				width: 110rpx;
				height: 110rpx;
			}
		}
	}
</style>
