<template>
	<view>
		<view class="content">
			<view class="main-title">
				<view class="cn">A+俱乐部</view>
				<view class="en">A+Club</view>
			</view>
			<view class="info">
				<view class="cu-form-group">
					<view class="title">输入框</view>
					<input v-model='login_form.check_num' placeholder="验证码" name="check_num"></input>
				</view>
				<view class="cu-form-group">
					<view class="title">验证码</view>
					<input v-model="login_form.phone" placeholder="请输入11位手机号" name="phone"></input>
					<button class='cu-btn bg-main shadow'>获取验证码</button>
				</view>
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

<style lang='scss'>
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
