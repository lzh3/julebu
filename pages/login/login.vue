<template>
	<view class="wrap">
		<view class="content">
			<view class="main-title">
				<view class="cn">AMD科技汇</view>
				<!-- <view class="en">A+Club</view> -->
			</view>
			<view class="info">
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/phone.png" mode=""></image>
					</view>
					<input v-model='login_form.phone' placeholder="请输入11位手机号" name="phone"></input>

					<button class='cu-btn bg-main shadow' data-target="Modal" @click="getCode">
						获取验证码 <text v-if="showTimer">({{timer}}s)</text>
					</button>
				</view>
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/safe.png" mode=""></image>
					</view>
					<input v-model="login_form.code" placeholder="请输入6位验证码" name="code"></input>
				</view>
				<view class="cu-form-group">
					<button class="login-btn bg-main" @click="login">登录</button>
				</view>
			</view>

		</view>
		<ke-fu></ke-fu>

		<Modal ref="modal" :msg='msg' :showlogin='showlogin' login_status='login' />
	</view>
</template>

<script>
	import KeFu from '../../components/Kefu/index.vue'
	import Modal from '../../components/Modal/index.vue'
	export default {
		data() {
			return {
				showTimer:false, // 是否显示倒计时
				timer:60, // 
				interval:null,
				showlogin: true,
				code: '',
				modal_title: '',
				info: 'yes',
				login_form: {
					phone: '', //15030017934
					code: ""
				},
				msg: '请输入正确信息!',
			}
		},
		components: {
			KeFu,
			Modal,
		},
		watch:{
			timer:function(val){
				if(val<=0){
					clearInterval(this.interval)
					this.timer=0;
				}
			}
		},
		methods: {
			getIdType(token) {
				let _this = this;
				if (token) {
					uni.request({
						url: 'https://amd.mcooks.cn/api/userinfo',
						method: 'get',
						header: {
							'authtoken': 'token ' + token,
						},
						success(res) {
							let id_type = 0 || (res.data.data && res.data.data.id_type);
							uni.setStorageSync('id_type', id_type)
							if(id_type!=2){
								uni.setTabBarItem({
									index:4,
									text:'',
									iconPath:'',
									success(res){
										console.log(res)
									}
								})
							}
						}
					})
				}
			},
			getCode(e) {
				// this.$refs.modal.open()
				this.showTimer=true;
				this.timer=setInterval(()=>{
					this.timer--;
				},1000)
				let _this = this;
				if (this.login_form.phone) {
					uni.request({
						url: 'https://amd.mcooks.cn/api/send/smscode',
						method: 'POST',
						data: {
							phone: this.login_form.phone
						},
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						success(res) {
							console.log(res.data)
							if (Number(res.data.code) === 200) {
								_this.code = res.data.code;
								uni.navigateTo({
									url: '../index/index'
								})
							} else {
								_this.fail_code = 0;
								_this.showlogin = false;
								_this.msg = res.data.msg;
								_this.$refs.modal.open()
							}
						},
						fail(e) {
							console.log(e)
						}
					})
				} else {
					this.msg = "请填写正确的手机号码";
					this.showlogin = false;
					this.$refs.modal.open()
				}
			},
			login() {
				// console.log(this.code)
				let _this = this;
				if (this.login_form.code) {
					uni.request({
						url: 'https://amd.mcooks.cn/api/login',
						method: 'POST',
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						data: this.login_form,
						success(res) {
							console.log(res.data)
							if (res.data.code === 200) {
								uni.setStorageSync('token', res.data.data.token);
								_this.getIdType(res.data.data.token)
								_this.showlogin = false;
								setTimeout(() => {
									uni.switchTab({
										url: '../index/index'
									})
								}, 500)
							} else {
								_this.showlogin = true;
							}
							_this.msg = res.data.msg;
							_this.modal_title = res.data.msg;
							_this.$refs.modal.open()
						},
						fail(e) {
							console.log(e)
						}
					})
				} else {
					this.msg = '请输入验证码'
					this.showlogin = false;
					this.$refs.modal.open()
				}
			}

		}
	}
</script>

<style lang='scss' scoped>
	.login-btn {
		width: 100%;
		height: 40px;
		margin: 8px 0;
		line-height: 40px;
		color: #fff;

	}

	.icon {
		width: 40rpx;
		height: 50rpx;
		vertical-align: bottom;
	}

	.wrap {
		padding-top: 1px;
	}

	.content {
		margin: 160rpx auto;

		.main-title {
			text-align: center;

			.cn {
				font-size: 32rpx;
			}

			.en {
				font-size: 22rpx;
				color: #bbb;
			}
		}

		.info {
			width: 90%;
			margin: 10px auto;
		}
	}
</style>