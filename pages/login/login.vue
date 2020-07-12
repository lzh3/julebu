<template>
	<view class="wrap">
		<view class="content">
			<view class="main-title">
				<view class="cn">A+俱乐部</view>
				<view class="en">A+Club</view>
			</view>
			<view class="info">
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/phone.png" mode=""></image>
					</view>
					<input v-model='login_form.check_num' placeholder="请输入11位手机号" name="check_num"></input>
				</view>
				<view class="cu-form-group">
					<view class="title">
						<image class="icon" src="../../static/image/icon/login_register/safe.png" mode=""></image>
					</view>
					<input v-model="login_form.phone" placeholder="请输入6位验证码" name="phone"></input>
					<button class='cu-btn bg-main shadow'>获取验证码</button>
				</view>
				<view class="cu-form-group"></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				login_form:{}
			}
		},
		methods: {

		}
	}
</script>

<style lang='scss' scoped>
	.icon{
		width:40rpx;
		height:50rpx;
		vertical-align: bottom;
	}
	.wrap{
		padding-top:1px;
	}
	.content{
		margin:160rpx auto;
		.main-title{
			text-align: center;
			.cn{
				font-size: 32rpx;
			}
			.en{
				font-size: 22rpx;
				color:#bbb;
			}
		}
		.info{
			width:90%;
			margin:10px auto;
		}
	}
</style>
