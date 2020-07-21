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
			<view class="out" @click='outLogin'>退出登录</view>
		</view>
		<view class="list">
			<view class="list-card bg-white">
				
				<view class="item" v-if="id_type!=1 && id_type!=2">
					<navigator url=''>
						<view class="img">
							<image src="../../static/image/icon/personal/1.png" mode=""></image>
						</view>
						<view class="txt">店铺详情</view>
					</navigator>
				</view>
				<view class="item"  v-if="id_type!=1 && id_type!=2">
					<navigator url=''>
						<view class="img">
							<image src="../../static/image/icon/personal/2.png" mode=""></image>
						</view>
						<view class="txt">店面形象</view>
					</navigator>
				</view>
				<view class="item"  v-if="id_type!=1  && id_type!=2">
					<navigator url=''>
						<view class="img">
							<image src="../../static/image/icon/personal/3.png" mode=""></image>
						</view>
						<view class="txt">店员管理</view>
					</navigator>
				</view>
				<view class="item" v-if="id_type!=4">
					<navigator url='./myactivity/myactivity'>
						<view class="img">
							<image src="../../static/image/icon/personal/4.png" mode=""></image>
						</view>
						<view class="txt">我的活动</view>
					</navigator>
				</view>
				<view class="item">
					<navigator url='./mytrain/mytrain'>
						<view class="img">
							<image src="../../static/image/icon/personal/5.png" mode=""></image>
						</view>
						<view class="txt">我的培训</view>
					</navigator>
				</view>
				<view class="item">
					<navigator url=''>
						<view class="img">
							<image src="../../static/image/icon/personal/6.png" mode=""></image>
						</view>
						<view class="txt">我的帖子</view>
					</navigator>
				</view>
				<view class="item">
					<navigator url='./myjf/myjf'>
						<view class="img">
							<image src="../../static/image/icon/personal/7.png" mode=""></image>
						</view>
						<view class="txt">我的积分</view>
					</navigator>
				</view>
				<view class="item">
					<navigator url=''>
						<view class="img">
							<image src="../../static/image/icon/personal/8.png" mode=""></image>
						</view>
						<view class="txt">积分奖励</view>
					</navigator>
				</view>
				<view class="item">
					<navigator url=''>
						<view class="img">
							<image src="../../static/image/icon/personal/9.png" mode=""></image>
						</view>
						<view class="txt">我的收藏</view>
					</navigator>
				</view>
				<view class="item">
					<navigator url=''>
						<view class="img">
							<image src="../../static/image/icon/personal/10.png" mode=""></image>
						</view>
						<view class="txt">我的客服</view>
					</navigator>
				</view>
			</view>
			
		</view>
		
		
		
		
		
		<!-- <view><navigator url="../login/login">去登录</navigator></view> -->
		<!-- <view class="reward">
			<view class="reward-title">
				<image src="../../static/image/personal/reward.png" mode=""></image>
				我的奖励
			</view>
		</view> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				token: '',
				username: '',
				id: '',
				id_type: 0, //  类型 1、注册用户 2、锐龙店面 3、渠道用户 4、CSR
				jf: '',
				avatar: '',
				type: '',
				user_info: {},
				/* list: [{
						image: '../../static/image/icon/personal/1.png',
						title: '店铺详情'
					},
					{
						image: '../../static/image/icon/personal/2.png',
						title: '店面形象'
					},
					{
						image: '../../static/image/icon/personal/3.png',
						title: '店员管理'
					},
					{
						image: '../../static/image/icon/personal/4.png',
						title: '我的活动',
						url: './myactivity/myactivity'
					},
					{
						image: '../../static/image/icon/personal/5.png',
						title: '我的培训',
						url: './mytrain/mytrain'
					},
					{
						image: '../../static/image/icon/personal/6.png',
						title: '我的帖子',
						url: './mypost/mypost'
					},
					{
						image: '../../static/image/icon/personal/7.png',
						title: '我的积分',
						url: './myjf/myjf'
					},
					{
						image: '../../static/image/icon/personal/8.png',
						title: '积分奖励'
					},
					{
						image: '../../static/image/icon/personal/9.png',
						title: '我的收藏',
						url: './mycollect/mycollect'
					},
					{
						image: '../../static/image/icon/personal/10.png',
						title: '我的客服'
					},
				] */
			}
		},
		async onShow() {
			let token = uni.getStorageSync('token');
			this.token = token;
			// console.log('==================',token)
			let id_type=uni.getStorageSync('id_type')
			this.id_type=id_type;
			await this.getPageInfo(token);
			await this.getUserInfo(token);
		},
		/* onShow() {
			
		}, */
		methods: {
			outLogin(){
				uni.removeStorageSync('token');
				uni.navigateTo({
					url:'../login/login'
				})
			},
			loginFn() {
				uni.navigateTo({
					url: '../login/login'
				})
			},
			// 获取页面信息
			getPageInfo(token) {
				if(token){
					uni.request({
						url: 'https://amd.mcooks.cn/api/user/modules',
						method: 'get',
						header: {
							'authtoken': 'token ' + token,
						},
						success(res) {
							console.log(res.data)
							// console.log("TCL: success -> res user/modules", res)
						}
					})
				}
			},
			// 获取个人信息
			getUserInfo(token) {
				let _this = this;
				if(token){
					uni.request({
						url: 'https://amd.mcooks.cn/api/userinfo',
						method: 'get',
						header: {
							'authtoken': 'token ' + token,
						},
						success(res) {
							console.log(res.data.data)
							let nickname ='' || (res.data.data&&res.data.data.nickname) ;
							uni.setStorageSync('username', nickname)
							if (res.data.code == 200) {
								const {
									nickname,
									id_type,
									id,
									reg_ip,
									realname,
									avatar,
								} = res.data.data
								_this.username = realname
								_this.id = id
								_this.jf = reg_ip
								_this.avatar = avatar
								switch (id_type) {
									case 1:
										_this.type = '注册用户'
										break;
									case 2:
										_this.type = '锐龙店面'
										break;
									case 3:
										_this.type = '渠道用户'
										break;
									case 4:
										_this.type = 'CSR'
										break;
								}
					
							}
					
						}
					})
				}
				
			}
		}
	}
</script>

<style lang="scss" scoped>
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
		.out{
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
