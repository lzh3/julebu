<template>
	<view>
		<view class="list">
			<view v-for="item in list" class="item">
				<navigator class="content" :url='item.url'>
					<view>{{item.a}}</view>
					<view>{{item.b}}</view>
				</navigator>
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
						b:'积分规则',
						url:'./RuleDetail'
					},
					{
						a:'CPU',
						b:'积分规则',
						url:'./RuleDetail'
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
			margin:20rpx 30rpx;
			text-align: center;
			color:#f36523;
			border: solid 1px #f36523;
			.content{
				display: block;
				width:100%;
				height:100%;
				padding-top:50rpx;
			}
		}
	}
	.ps{
		margin:30rpx 10px;
		font-size: 24rpx;
	}
</style>
