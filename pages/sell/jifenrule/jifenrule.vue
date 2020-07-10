<template>
	<view>
		<view class="list">
			<view v-for="item in list" class="item">
				<view>{{item.a}}</view>
				<view>{{item.b}}</view>
			</view>
		</view>
	
		<view class="ps">
			注：积分规则与兑换产品最终解释权归AMD公司所有
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[
					{
						a:'CPU',
						b:'积分规则',
						url:'./RuleDetail'
					},
					{
						a:'CPU',
						b:'积分规则',
						url:'./RuleDetail'
					},
					{
						a:'主板',
						b:'积分规则'
					},
					{
						a:'CPU',
						b:'积分规则'
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
		display: flex;
		width:500rpx;
		margin:0 auto;
		justify-content: space-around;
		flex-wrap: wrap;
		.item{
			width:160rpx;
			height:160rpx;
			padding-top:50rpx;
			margin:20rpx 30rpx;
			text-align: center;
			color:#f36523;
			border: solid 1px #f36523;
		}
	}
	.ps{
		margin:30rpx 10px;
		font-size: 24rpx;
	}
</style>
