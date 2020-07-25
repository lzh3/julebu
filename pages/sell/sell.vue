<template>
	<view class="bg-white container border">

		<view class="list"  v-if='id_type==2'>
			<view v-for="item in list" class="item">
				<!-- <navigator :url='item.url'> -->
				<view>
					<view class="img">
						<image :src="item.pic" mode=""></image>
					</view>
					<view class="txt">{{item.detail}}</view>
				</view>
				<!-- </navigator> -->
			</view>
		</view>
		<view v-else class="no-permision">
			权限暂未开放
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id_type: 1,
				list: [{
						detail: 'CPU销售',
						url: './jifenrule/jifenrule',
						pic: '../../static/image/sell/1.png'
					},
					{
						detail: '显卡销售',
						url: './jifenrule/jifenrule',
						pic: '../../static/image/sell/2.png'
					},
					{
						detail: '主板销售',
						url: './jifenrule/jifenrule',
						pic: '../../static/image/sell/3.png'
					},
					{
						detail: '优选销售',
						url: './jifenrule/jifenrule',
						pic: '../../static/image/sell/4.png'
					},
					{
						detail: '积分规则',
						//url: '../register/register',
						url: '../mustlook/mustlook',
						pic: '../../static/image/sell/5.png'
					},
				]
			}
		},
		onLoad() {
			let token = uni.getStorageSync('token');
			this.getUserInfo(token)
		},
		methods: {
			// 用于判断用户是否有权限查看
			getUserInfo(token) {
				let _this = this;
				if (token) {
					uni.request({
						url: "https://amd.mcooks.cn/api/userinfo",
						method: "get",
						header: {
							authtoken: "token " + token,
						},
						success(res) {
							if (res.data.code == 200) {
								const {
									id_type,
								} = res.data.data;
								_this.id_type = id_type;
							}
						},
					});
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.border{
		border-top:1px solid transparent;
	}
	.no-permision {
		width: 400rpx;
		font-size: 40rpx;
		text-align: center;
		margin: 40rpx auto;
	}

	.list {
		width: 750rpx;
		margin: 0 auto;
		padding: 0 15rpx;
		display: flex;
		flex-wrap: wrap;

		.item {
			width: 180rpx;
			height: 180rpx;
			margin: 20rpx 30rpx 0;
			text-align: center;

			.img {
				width: 80rpx;
				height: 80rpx;
				margin: 0 auto;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.txt {
				margin-top: 5px;
			}
		}
	}
</style>
