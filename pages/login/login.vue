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
					<input v-model='login_form.phone' placeholder="请输入11位手机号" name="phone"></input>
					
					<button class='cu-btn bg-main shadow'>获取验证码</button>
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
	</view>
</template>

<script>
	import KeFu from '../../components/Kefu/index.vue'
	export default {
		data() {
			return {
				login_form:{}
			}
		},
		components:{
			KeFu,
		},
		methods: {
			login(){
				console.log(this.login_form)
				uni.request({
					url:'/login',
					data:this.login_form,
					success(res){
						console.log(res)
					}
				})
			}
		}
	}
</script>

<style lang='scss' scoped>
	.login-btn{
		width:100%;
		height:40px;
		margin:8px 0;
		line-height: 40px;
		color:#fff;
		
	}
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
