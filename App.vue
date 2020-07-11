<script>
	export default {
		onLaunch: function () {
			console.log('App Launch');
		},
		onShow: function () {
			console.log('App Show');
		},
		onHide: function () {
			console.log('App Hide');
		}
	};
</script>

<style>
	@import "colorui/main.css";
	@import "colorui/icon.css";

	.main-color {
		color: #f36523;
	}

	.wrap {
		width: 100%;
		height: 100vh;
		background-color: #fff;
	}

	.bg-main {
		background-color: #f36523;
		color: #fff;
	}

	.container {
		width: 100%;
		height: 100vh;
		background-color: #F5F5F5;
	}

	.bg-white {
		background-color: #fff;
	}

	/* 解决头条小程序组件内引入字体不生效的问题 */
	/* #ifdef MP-TOUTIAO */
	@font-face {
		font-family: uniicons;
		src: url('/static/uni.ttf');
	}

	/* #endif */
</style>