<template>
	<view class="reg">
		<view class="content">
			<view class="main-title">
				<view class="cn">AMD科技汇</view>
				<!-- <view class="en">A+Club</view> -->
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
					<!-- <input v-model='reg_form.address' placeholder="请输入公司地址,例:省-市-区/县-详细地址" name="check_num"></input> -->
					<view id="pickpick">
						 <picker-address @change="bindPickerChange">{{txt}}</picker-address>
					</view>
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

					<button class='cu-btn bg-main shadow' @tap='getCode'>
						获取验证码
						<text v-if="showTimer">({{timer}}s)</text>
					</button>
				</view>
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/safe.png" mode=""></image>
					</view>
					<input v-model="reg_form.code" placeholder="请输入6位验证码" name="phone"></input>
				</view>
				<view class="cu-form-group">
					<view class="uni-title">是否愿意加入amd体系</view>
					<view class="uni-padding-wrap">
						<radio-group>
							<label class="radio">
								<radio value="0" v-model="reg_form.is_join_amd" />是
							</label>
							<label class="radio">
								<radio value="1" v-model="reg_form.is_join_amd" />否
							</label>
						</radio-group>
					</view>
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
			</view>
		</view>

		<Modal ref="reg" :showLogin='showLogin' :reg_status='reg_status' :msg='msg' />
		<Modal ref="pp" :code_status='code_status' :msg='msg' />
	</view>
</template>

<script>
	import Modal from '../../components/Modal/index.vue'
	import PickerAddress from '../../components/pickerAddress/pickerAddress.vue'
	export default {
		data() {
			return {
				txt:'',
				showTimer: false, // 是否显示倒计时
				timer: 60, // 
				interval: null,
				msg: '请输入正确信息!',
				code: '',
				showLogin: false,
				reg_status: 'reg_error',
				code_status: 'code_error',
				reg_form: {
					connect: '联系', //人
					"company": "公司", // 名称 asdasd
					"province": "湖南省长沙市1区", // 省 河北省
					"city": "", // 市 邯郸市
					"address": "", // 详细地址 湖南省长沙市雨花区
					"business": "阿发达", // 公司主营行业 公司主营行业
					"realname": "1士大夫撒旦", // 联系人 联系人
					"phone": "18270825620", // 联系手机 18270825622
					"code": "", // 验证码 816726
					is_join_amd: "0",
				}
			}
		},
		components: {
			Modal,
			PickerAddress
		},
		watch: {
			timer: function(val) {
				if (val <= 0) {
					clearInterval(this.interval)
					this.timer = 0;
				}
			}
		},
		methods: {
			bindPickerChange(e){
				console.log(e)
				 this.index = e.target.value
			},
			regFn() {
				// console.log(this.reg_form)
				let _this = this;
				let {
					address,
					business,
					code,
					company,
					connect,
					phone,
					is_join_amd
				} = this.reg_form;
				var reg = /.+?(省|市|自治区|自治州|县|区)/g;
				//console.log(this.reg_form.address.match(reg))

				let regRes = this.reg_form.address.match(reg) || [];

				if (address && business && code && company && connect && phone && is_join_amd) {
					uni.request({
						url: 'https://amd.mcooks.cn/api/register',
						method: 'POST',
						data: {
							company, // 公司名称
							"province": regRes[0], // 省
							"city": regRes[1], // 市
							"district": regRes[2],
							address, // 详细地址
							business, // 公司主营行业
							"realname": connect, // 联系人
							phone, // 联系手机
							code, // 验证码
							is_join_amd: Number(is_join_amd)
						},
						success(res) {
							console.log(res)
							_this.msg = res.data.msg;
							_this.reg_status = 'reg_error';
							_this.$refs.reg.open()

						},
						fail(e) {
							console.log(e)
						}
					})
				} else {
					_this.showLogin = false;
					_this.$refs.reg.open()
				}
			},
			getCode() {
				this.showTimer = true;
				this.interval = setInterval(() => {
					this.timer--;
				}, 1000)

				// this.$refs.modal.open()
				let _this = this;
				uni.request({
					url: 'https://amd.mcooks.cn/api/send/smscode',
					method: 'POST',
					data: {
						phone: this.reg_form.phone
					},
					success(res) {
						console.log(res.data)
						if (Number(res.data.code) === 200) {
							_this.code = res.data.code
							/* uni.navigateTo({
								url:'/pages/login/login'
							}) */
						} else {
							_this.msg = res.data.msg;
							_this.code_status = 'code_error';
							_this.$refs.pp.open()
						}
					},
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	#pickpick{
		width:100px ;
		height:15px; 
		background-color: skyblue;
	}
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
