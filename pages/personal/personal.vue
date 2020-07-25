<template>
	<view class="container">
		<view class="info-card">
			<view class="card-bg-img">
				<image src="../../static/image/personal/bg.png" mode=""></image>
			</view>
			<view class="info">
				<view class="touxiang">
					<!-- -->
					<image v-if="avatar==''" src="../../static/image/headimg.png" mode=""></image>
					<image :src="avatar" v-else mode=""></image>
				</view>
				<view class="status">
					<view class="name">
						{{username}} ID:{{id}}
					</view>
					<view class="jifen">
						积分：{{jf}}
					</view>
					<view class="login" @click="loginFn" v-if="token==''">立即登录</view>
					<view class="type" v-else>{{type}}</view>
				</view>
			</view>
			<view class="out" @click='outLogin' v-if="token">退出登录</view>
		</view>
		<!-- 模块展示 -->
		<view class="list">
			<view class="list-card bg-white">
				<template v-for="item in moduleList">
					<view class="item" v-if="moduleRight(item)">
						 <view class="kefu" v-if="item.title=='我的客服'">
							<button open-type="contact" class="btn"></button>
							<view class="img">
								<image :src="item.image" />
							</view>
							<text>{{item.title}}</text>
						</view> 
						<!-- 打开类型 1、小程序 2、H5 -->
						<navigator v-else :url="item.type===1 ? item.url : `/pages/webview/webview?url=${item.url}`">
							<view class="img">
								<image :src="item.image" />
							</view>
							<view class="txt">{{item.title}}</view>
						</navigator>
					</view>
				</template>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				token: "",
				username: "",
				id: "",
				id_type: 0, //  类型 1、注册用户 2、锐龙店面 3、渠道用户 4、CSR
				jf: "",
				avatar: "",
				type: "",
				user_info: {},
				moduleList: [{
						image: '../../static/image/icon/personal/1.png',
						title: '店铺详情',
						url:'',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/2.png',
						title: '店面形象',
						url:'',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/3.png',
						title: '店员管理',
						url:'',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/4.png',
						title: '我的活动',
						url: './myactivity/myactivity',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/5.png',
						title: '我的培训',
						url: './mytrain/mytrain',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/6.png',
						title: '我的帖子',
						url: './mypost/mypost',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/7.png',
						title: '我的积分',
						url: './myjf/myjf',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/8.png',
						title: '积分奖励',
						url:'',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/9.png',
						title: '我的收藏',
						url: './mycollect/mycollect',
						type: 1,
					},
					{
						image: '../../static/image/icon/personal/10.png',
						title: '我的客服',
						url:'',
						type: 1,
					},
				],
			};
		},
		async onShow() {
			let token = uni.getStorageSync("token");
			this.token = token;
			let id_type = uni.getStorageSync("id_type");
			this.id_type = id_type;
			await this.getPageInfo(token);
			await this.getUserInfo(token);
		},
		methods: {
			moduleRight(item) {
				let title = item.title; //模块名
				let right = [
					// 1、注册用户
					[
						"我的收藏",
						"我的活动",
						"我的培训",
						"我的帖子",
						"我的积分",
						"积分奖励",
						"我的客服",
					],
					// 2、锐龙店面
					[
						"我的收藏",
						"我的活动",
						"我的培训",
						"我的帖子",
						"我的积分",
						"积分奖励",
						"我的客服",
					],
					// 3、渠道用户
					[
						"我的收藏",
						"我的活动",
						"我的培训",
						"我的帖子",
						"店面详情",
						"店面形象",
						"店员管理",
						"我的积分",
						"积分规则",
						"积分奖励",
						"联系客服",
					],
					// 4、CSR
					[
						"我的收藏",
						"我的培训",
						"我的帖子",
						"积分规则",
						"店铺详情",
						"业绩管理",
						"形象管理",
						"考试管理",
						"条码查询",
						"联系客服",
					],
				];
				switch (this.id_type) {
					case 1:
						return right[0].includes(title);
						break;
					case 2:
						return right[1].includes(title);
						break;
					case 3:
						return right[2].includes(title);
						break;
					default:
						return right[3].includes(title);
						//4
						break;
				}
			},
			outLogin() {
				uni.removeStorageSync("token");
				uni.navigateTo({
					url: "../login/login",
				});
			},
			loginFn() {
				uni.navigateTo({
					url: "../login/login",
				});
			},
			// 获取页面信息
			getPageInfo(token) {
				if (token) {
					let _this = this;
					uni.request({
						//url: '/user/modules',
						url: "https://amd.mcooks.cn/api/user/modules",
						method: "get",
						header: {
							authtoken: "token " + token,
						},
						success({
							data
						}) {
							console.log("TCL: success -> data", data);
							_this.moduleList = data.data;
						},
					});
				}
			},
			// 获取个人信息
			getUserInfo(token) {
				let _this = this;
				if (token) {
					uni.request({
						url: "https://amd.mcooks.cn/api/userinfo",
						//url: '/userinfo',
						method: "get",
						header: {
							authtoken: "token " + token,
						},
						success(res) {
							let nickname = "" || (res.data.data && res.data.data.nickname);
							uni.setStorageSync("username", nickname);
							if (res.data.code == 200) {
								const {
									nickname,
									id_type,
									id,
									reg_ip,
									realname,
									avatar,
								} = res.data.data;
								_this.username = realname;
								_this.id = id;
								_this.jf = reg_ip;
								_this.avatar = avatar;
								_this.type = id_type == 1 ? "注册用户" : "非注册用户";
							}
						},
					});
				}
			},
		},
	};
</script>

<style lang="scss" scoped>
	.kefu {
		position: relative;
	
		.btn {
			display: block;
			position: absolute;
			top:0;
			left:0;
			width:146rpx;
			height:142rpx;
			z-index: 2;
			border: none;
			opacity: 0;
			outline: none;
		}
	}
	.reward {
		.reward-title {
			width: 100%;
			height: 60rpx;
			line-height: 60rpx;
			text-align: center;
			background-color: #fff;

			image {
				width: 40rpx;
				height: 40rpx;
				vertical-align: middle;
			}
		}
	}

	.list {
		width: 100%;
		height: 362rpx;
		position: relative;

		.list-card {
			display: flex;
			display: -webkit-flex;
			position: absolute;
			align-items: center;
			flex-wrap: wrap;
			flex-direction: row;
			width: 690rpx;
			top: -100rpx;
			left: 50%;
			margin-left: -345rpx;
			border-radius: 10px;
			padding: 10rpx;
			box-sizing: border-box;
			overflow: hidden;

			.item {
				width: 147.5rpx;
				height: 142rpx;
				padding: 8rpx 0;
				margin: 2rpx 10rpx;
				box-sizing: border-box;
				text-align: center;
				color: #f39d23;

				.img {
					width: 76rpx;
					height: 76rpx;
					margin: 0 auto;

					image {
						width: 100%;
						height: 100%;
					}
				}
			}
		}
	}

	.info-card {
		position: relative;
		z-index: 0;
		width: 100%;
		height: 274rpx;
		padding: 15rpx 30rpx;
		box-sizing: border-box;
		font-size: 24rpx;
		// background:url(../../static/image/personal/bg.png) no-repeat fixed top;
		background-size: 100% 342rpx;
		color: #fff;

		.card-bg-img {
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			height: 100%;
			z-index: -1;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.touxiang {
			width: 98rpx;
			height: 98rpx;
			float: left;
			border-radius: 50%;
			background-color: #fff;
			overflow: hidden;
			text-align: center;

			image {
				width: 100%;
				height: 100%;
				vertical-align: middle;
			}
		}

		.status {
			float: left;
			margin: 10rpx;

			.jifen {
				margin-top: 3px;
			}
		}

		.login {
			position: absolute;
			width: 145rpx;
			height: 48rpx;
			line-height: 48rpx;
			text-align: center;
			right: 30rpx;
			top: 15rpx;
			color: #fff;
			font-size: 28rpx;
			border-radius: 10rpx;
			background-color: #f39d23;
		}

		.out {
			position: absolute;
			width: 145rpx;
			height: 48rpx;
			line-height: 48rpx;
			text-align: center;
			right: 30rpx;
			top: 80rpx;
			color: #fff;
			font-size: 28rpx;
			border-radius: 10rpx;
			background-color: #f39d23;
		}

		.type {
			position: absolute;
			line-height: 48rpx;
			text-align: center;
			right: 30rpx;
			top: 15rpx;
			color: #fff;
			font-size: 28rpx;
			border-radius: 10rpx;
		}
	}
</style>
