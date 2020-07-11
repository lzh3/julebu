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

	/* noto-sans-sc-regular */
	@font-face {
		font-family: 'Noto Sans SC';
		font-style: normal;
		font-weight: regular;
		src: url('//lib.baomitu.com/fonts/noto-sans-sc/noto-sans-sc-regular.eot');
		/* IE9 Compat Modes */
		src: local('Noto Sans SC'), local('NotoSans SC-Normal'),
			url('//lib.baomitu.com/fonts/noto-sans-sc/noto-sans-sc-regular.eot?#iefix') format('embedded-opentype'),
			/* IE6-IE8 */
			url('//lib.baomitu.com/fonts/noto-sans-sc/noto-sans-sc-regular.woff2') format('woff2'),
			/* Super Modern Browsers */
			url('//lib.baomitu.com/fonts/noto-sans-sc/noto-sans-sc-regular.woff') format('woff'),
			/* Modern Browsers */
			url('//lib.baomitu.com/fonts/noto-sans-sc/noto-sans-sc-regular.ttf') format('truetype'),
			/* Safari, Android, iOS */
			url('//lib.baomitu.com/fonts/noto-sans-sc/noto-sans-sc-regular.svg#NotoSans SC') format('svg');
		/* Legacy iOS */
	}

	html,
	body {
		font-family: 'Noto Sans SC', sans-serif;
	}

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