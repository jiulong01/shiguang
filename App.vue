<script>
	export default {
		globalData: {
			isDarkMode: false,
		},
		data() {
			return {};
		},
		onLaunch: function() {
			console.log("App Launch");
			// 检查本地存储中的模式偏好
			const darkMode = uni.getStorageSync("darkMode");
			if (darkMode !== "") {
				this.globalData.isDarkMode = darkMode;
				this.updateGlobalStyle();
			}
		},
		onShow: function() {
			console.log("App Show");
		},
		onHide: function() {
			console.log("App Hide");
		},
		methods: {
			// 切换深色/亮色模式
			toggleDarkMode() {
				this.globalData.isDarkMode = !this.globalData.isDarkMode;
				// 保存模式偏好到本地存储
				uni.setStorageSync("darkMode", this.globalData.isDarkMode);
				this.updateGlobalStyle();
			},
			// 更新全局样式
			updateGlobalStyle() {
				if (this.globalData.isDarkMode) {
					document.body.classList.add("dark-mode");
				} else {
					document.body.classList.remove("dark-mode");
				}
			},
		},
	};
</script>

<style>
	/*每个页面公共css */
	/* 全局深色模式样式 */
	.dark-mode {
		--uni-bg-color: #1a1a1a;
		--uni-bg-color-grey: #2c2c2c;
		--uni-text-color: #ffffff;
		--uni-text-color-inverse: #000000;
		--uni-text-color-grey: #aaaaaa;
		--uni-border-color: #333333;
	}
</style>