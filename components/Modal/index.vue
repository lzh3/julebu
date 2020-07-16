<template>
	<!-- 弹窗 -->
	<uni-popup ref="popup" type="center">
		<view class="modal-wrap error" v-show="status === 'error'">
			<image src="../../static/image/error.png" class="error" />
			<text class="title">{{title}}</text>
			<text class="desc" v-if="$slots.desc">
				<slot name="desc"></slot>
			</text>
			<text class="desc" v-else>{{desc}}</text>
			<button type="primary" @click="fn" class="btn">{{btnText}}</button>
			<image src="../../static/image/close.png" class="close" @click="close" />
		</view>
		<view class="modal-wrap success" v-show="status === 'success'">
			<image src="../../static/image/success-line.png" class="success" />
			<text class="title">{{title}}</text>
			<text class="desc" v-if="$slots.desc">
				<slot name="desc"></slot>
			</text>
			<text class="desc" v-else>{{desc}}</text>
			<button type="primary" @click="fn" class="btn" v-if="$attrs.btn">{{btnText}}</button>
			<image src="../../static/image/close.png" class="close" @click="close" />
		</view>
		<view class="login-wrap modal-wrap success" v-show="status === 'login'">
			<text class="title login text-green" :class="isSuccess?'login_suc':'login_err'">{{msg}}</text>
			<button type="default" v-show='showlogin' class="btn reg-btn" @tap='toRegister'>去注册</button>
		</view>
		<view class="modal-wrap error" v-show="status === 'reg-error'">
			<text class="title">{{msg}}</text>
			<button type="primary" @click="loginFn" class="btn">去登录</button>
			<image src="../../static/image/close.png" class="close" @click="close" />
		</view>
	</uni-popup>
	</view>
</template>

<script>
	export default {
		/* success:{
		  default:'成功',
		  type:String
	  }, */
		props: {
			msg: {
				default: () => '请输入正确信息'
			},
			showlogin:{
				default: () => false
			},
			isSuccess: {
				default: () => 'login_suc'
			},
			status: {
				default: () => 'error'
			},
			btnText: {
				default: () => '立即登录'
			},
			desc: {
				default: () => '你还未登录,请登录进行报名'
			},
			title: {
				default: () => '参与失败'
			},
			cb: {
				default: () => () => {}
			},
		},
		data() {
			return {

			};
		},
		mounted() {
			console.log("TCL: onLoad -> this", this)
		},
		methods: {
			loginFn(){
				uni.navigateTo({
					url: '../../pages/login/login'
				})
				this.$refs.popup.close();
			},
			toRegister() {
				uni.navigateTo({
					url: '../../pages/register/register'
				})
				this.$refs.popup.close();
			},
			close() {
				this.$refs.popup.close()
			},
			open() {
				this.$refs.popup.open()
			},
			fn() {
				this.cb()
				uni.navigateTo({
					url: '../../pages/personal/mytrain/mytrain'
				})
			}
		},

	};
</script>

<style scoped lang='scss'>
	.reg-btn {
		color:#fff;
	} 

	.login-wrap {
		width: 438rpx;
		// height: 300rpx;
		min-height: 200rpx;
		background: rgba(255, 255, 255, 1);
		border-radius: 30rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		position: relative;

		.login {
			font-size: 36rpx;
			font-weight: bold;
			line-height: 200rpx;

			&.login_err {
				color: red;
			}
		}
	}

	//弹窗
	.modal-wrap {
		width: 438rpx;
		// height: 300rpx;
		min-height: 200rpx;
		background: rgba(255, 255, 255, 1);
		border-radius: 30rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		padding-bottom: 30rpx;
		position: relative;

		.error {
			width: 80rpx;
			min-width: 80rpx;
			height: 80rpx;
			min-height: 80rpx;
			margin: 20rpx 0;
		}

		.success {
			width: 70rpx;
			min-width: 70rpx;
			height: 70rpx;
			min-height: 70rpx;
			margin: 20rpx 0;
		}

		.close {
			width: 40rpx;
			height: 40rpx;
			position: absolute;
			z-index: 99999;
			bottom: -50rpx;
		}

		.title {
			font-size: 30rpx;
			font-weight: bold;
			color: rgba(51, 51, 51, 1);
		}

		.desc {
			line-height: 1;
			font-size: 24rpx;
			font-weight: 500;
			margin: 20rpx 30rpx;
			color: rgba(153, 153, 153, 1);
		}

		.btn {
			box-sizing: content-box;
			width: 145rpx;
			min-height: 48rpx;
			line-height: 48rpx;
			background: rgba(243, 101, 35, 1);
			border-radius: 10rpx;
			font-size: 28rpx;
			text-align: center;
		}
	}
</style>
