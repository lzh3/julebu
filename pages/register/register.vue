<template>
	<view class="reg">
		<view class="content">
			<view class="main-title">
				<view class="cn">A+俱乐部</view>
				<view class="en">A+Club</view>
			</view>
			<view class="info">
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/company.png" mode=""></image>
					</view>
					<input v-model='reg_form.company' placeholder="请输入公司名称" name="check_num"></input>
				</view>
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/address.png" mode=""></image>
					</view>
					<input v-model='reg_form.address' placeholder="请输入公司地址,例:省-市-区/县-详细地址" name="check_num"></input>
				</view>
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/range.png" mode=""></image>
					</view>
					<input v-model='reg_form.business' placeholder="请输入主营业务" name="check_num"></input>
				</view>
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/connect.png" mode=""></image>
					</view>
					<input v-model='reg_form.connect' placeholder="请输入联系人" name="check_num"></input>
				</view>
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/phone.png" mode=""></image>
					</view>
					<input v-model='reg_form.phone' placeholder="请输入11位手机号" name="check_num"></input>

					<button class='cu-btn bg-main shadow' @tap='getCode'>获取验证码</button>
				</view>
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/safe.png" mode=""></image>
					</view>
					<input v-model="reg_form.code" placeholder="请输入6位验证码" name="phone"></input>
				</view>
				<view class="cu-form-group">
					<button class="reg-btn bg-main" @click="regFn">注册</button>
				</view>
				<!-- <view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/phone.png" mode=""></image>
					</view>
					<input v-model='login_form.check_num' placeholder="请输入6位验证码" name="check_num"></input>
				</view> -->
				<Modal ref="pp" status='reg-error' :msg='msg'/>
			</view>
		</view>
	</view>
</template>

<script>
	import Modal from '../../components/Modal/index.vue'
	export default {
		data() {
			return {
				msg:'请输入正确信息!',
				reg_form: {

				}
			}
		},
		components: {
			Modal
		},
		methods: {
			regFn() {
				console.log(this.reg_form)
				let {
					address,
					business,
					code,
					company,
					connect,
					phone
				} = this.reg_form;
				var reg = /.+?(省|市|自治区|自治州|县|区)/g;
				//console.log(this.reg_form.address.match(reg))

				let regRes = [];
				if (!regRes) {
					regRes = this.reg_form.address.match(reg)
				}
				if (address && business && code && company && connect && phone) {
					uni.request({
						url: '/register',
						data: {
							company, // 公司名称
							"province": regRes[0], // 省
							"city": regRes[1], // 市
							address, // 详细地址
							businness, // 公司主营行业
							"realname": connect, // 联系人
							phone, // 联系手机
							code // 验证码
						}
					})
				} else {
					this.$refs.pp.open()
				}
			},
			getCode(){
				// this.$refs.modal.open()
				let _this=this;
				uni.request({
					url:'/send/smscode',
					method:'POST',
					data:{
						phone:this.reg_form.phone
					},
					success(res){
						if(Number(res.data.code)===200){
							_this.code=res.data.code
							uni.navigateTo({
								url:'/pages/login/login'
							})
						}else{
							_this.$refs.pp.open()
						}
					},
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.reg-btn {
		width: 100%;
		height: 40px;
		margin: 8px 0;
		line-height: 40px;
		color: #fff;
	}

	.reg {
		width: 100%;
		height: 100vh;
		margin: 0;
		padding: 0;
		background-color: #fff;
	}

	.icon {
		width: 40rpx;
		height: 40rpx;
		vertical-align: bottom;
	}

	.wrap {
		padding-top: 1px;
	}

	.content {
		padding: 160rpx 0 0;

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
