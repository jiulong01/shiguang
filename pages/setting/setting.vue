<template>
	<view class="setting">
		<view class="setting-content">
			<!-- 标题 -->
			<view class="setting-title">设置</view>
			<!-- 个人账号 -->
			<view class="setting-mine" @click="toVip">
				<view><img src="../../static/task/task-avatar.png" alt="" class="setting-mine-img" /></view>
				<view class="setting-mine-txt">
					<view class="txt1">Alex
						<!-- <uni-icons type="vip-filled" size="20" color="#fde047"></uni-icons> -->
						<img src="@/static/vip/vipLogo.png" class="txt1-vip" alt="" />
					</view>
					<view class="txt2">高级会员</view>
				</view>
				<view class="setting-mine-icon"><uni-icons type="right" size="30" color="#fff"></uni-icons></view>
			</view>
			<!-- 账号与安全 -->
			<view class="account">
				<view class="account-txt">账号与安全</view>
				<!-- 个人信息 -->
				<view class="account-content flex" @click="toPersonaInfo">
					<view class="content-icon"><uni-icons type="person" size="20" color="#fca185"></uni-icons></view>
					<view class="content-txt">个人信息</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
				<!-- 账号安全 -->
				<view class="account-content flex" @click="Unfinished">
					<view class="content-icon"><uni-icons type="checkbox" size="20" color="#4caf50"></uni-icons></view>
					<view class="content-txt">账号安全</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
				<!-- 隐私设置 -->
				<view class="account-content flex" @click="Unfinished">
					<view class="content-icon"><uni-icons type="locked" size="20" color="#55aef6"></uni-icons></view>
					<view class="content-txt">隐私设置</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
			</view>
			<!--功能设置 -->
			<view class="function">
				<!-- 功能设置 -->
				<view class="account-txt">功能设置</view>
				<!-- 通知设置 -->
				<view class="account-content flex" @click="toNotification">
					<link rel="stylesheet" href="//at.alicdn.com/t/c/font_4925127_amga74829y.css" />
					<view class="content-icon"><uni-icons color="#fca185">
							<i class="iconfont icon-tongzhi"></i></uni-icons></view>
					<view class="content-txt">通知设置</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
				<!-- 时间通知 -->
				<view class="account-content flex" @click="Unfinished">
					<view class="content-icon"><uni-icons color="#4caf50">
							<i class="iconfont icon-naozhong"></i></uni-icons></view>
					<view class="content-txt">时间通知</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>

			</view>
			<view class="function">
				<!-- 功能设置 -->
				<view class="account-txt">数据与云服务</view>
				<!-- 数据同步 -->
				<view class="account-content flex">
					<view class="content-icon"><uni-icons color="#fca185">
							<i class="iconfont icon-shujutongbu2"></i></uni-icons></view>
					<view class="content-txt">数据同步</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
				<!-- 四象限统计 -->
				<view class="account-content flex">
					<view class="content-icon"><uni-icons color="#55aef6">
							<i class="iconfont icon-stick"></i></uni-icons></view>
					<view class="content-txt">四象限统计</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
			</view>
			<view class="function">
				<view class=""></view>
				<!-- 关于我们 -->
				<view class="account-content flex" @click="Unfinished">
					<view class="content-icon"><uni-icons color="#fca185">
							<i class="iconfont icon-gantanhao"></i></uni-icons></view>
					<view class="content-txt">关于我们</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
				<!-- 帮助与反馈 -->
				<view class="account-content flex" @click="Unfinished">
					<view class="content-icon"><uni-icons color="#4caf50">
							<i class="iconfont icon-wenhao-xianxingyuankuang"></i></uni-icons></view>
					<view class="content-txt">帮助与反馈</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
				<!-- 清除缓存 -->
				<view class="account-content flex" @click="Unfinished">
					<view class="content-icon"><uni-icons color="#55aef6">
							<i class="iconfont icon-lajitong"></i></uni-icons></view>
					<view class="content-txt">清除缓存</view>
					<view class="content-icon2"><uni-icons type="right" size="20" color="#999999"></uni-icons></view>
				</view>
			</view>
			<!-- 退出登录 -->
			<view class="Close-login flex" @click="LogOut"><uni-icons type="download" size="20"
					color="#ff4d4f"></uni-icons>退出登录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {};
		},
		methods: {
			toVip() {
				uni.navigateTo({
					url: "/pages/toVip/toVip"
				})
			},
			toPersonaInfo() {
				uni.navigateTo({
					url: "/pages/Personalnfo/Personalnfo"
				})
			},
			toNotification() {
				uni.navigateTo({
					url: "/pages/NotificationSettings/NotificationSettings"
				})
			},
			Unfinished() {
				uni.showToast({
					icon: 'none',
					title: "功能建设中~"
				})
			},
			LogOut() {
				uni.showToast({
					icon: "success",
					title: "已退出",
					complete: () => {
						setTimeout(() => {
							uni.redirectTo({
								url: "/pages/login/login",
							});
						}, 1500); // 延迟1.5秒跳转，确保用户看到提示
					},
				});
			},
		},
	};
</script>

<style scoped>
	/* 全局 */
	.setting {
		display: flex;
		justify-content: center;
		min-height: 100vh;
		padding-left: constant(safe-area-inset-left);
		padding-right: constant(safe-area-inset-right);
		padding-left: env(safe-area-inset-left);
		padding-right: env(safe-area-inset-right);
		/* padding-right: 35rpx; */
	}

	/* 标题 */
	.setting-title {
		width: 100%;
		height: 50rpx;
		padding: 70rpx 0rpx 30rpx;
		font-size: 35rpx;
	}

	/* 个人账户 */
	.setting-mine {
		width: 620rpx;
		height: 200rpx;
		border-radius: 20rpx;
		/* padding-left: 20rpx; */
		display: flex;
		/* justify-content: center; */
		align-items: center;
		padding: 0rpx 30rpx;
		background-image: linear-gradient(221deg, #fec163 15%, #de4313 90%);
	}

	.setting-mine-img {
		width: 100rpx;
		height: 100rpx;
		border-radius: 50%;
		background-color: #ddd;
		margin-right: 20rpx;
	}

	.setting-mine-txt {
		flex: 1;
		/* color: white; */
	}

	.setting-mine-txt .txt1 {
		font-size: 35rpx;
		color: aliceblue;
	}

	.setting-mine-txt .txt1 .uni-icons {
		margin-left: 20rpx;
	}

	.txt1-vip {
		margin-left: 14rpx;
		width: 44rpx;
		height: 30rpx;
	}

	.setting-mine-txt .txt2 {
		font-size: 28rpx;
		color: #ddd;
	}

	.setting-mine-icon {
		width: 40rpx;
		height: 40rpx;
		color: aliceblue;
	}

	/* 账号与安全 */
	.account {
		margin-top: 50rpx;
	}

	.account-txt {
		font-size: 32rpx;
		margin-bottom: 25rpx;
	}

	.account-content {
		/* width: 100%; */
		height: 100rpx;
		align-items: center;
		padding: 0rpx 30rpx;
		background-color: #f1f5f9;
	}

	.account-content:nth-child(2) {
		border-top-right-radius: 20rpx;
		border-top-left-radius: 20rpx;
	}

	.account-content:last-child {
		border-bottom-right-radius: 20rpx;
		border-bottom-left-radius: 20rpx;
	}

	.account-content:nth-child(4) {
		border-bottom-right-radius: 20rpx;
		border-bottom-left-radius: 20rpx;
	}

	.account-content:nth-child(3) .content-icon {
		background-color: #ddece4;
	}

	.account-content:nth-child(4) .content-icon {
		background-color: #dbebf9;
	}

	.content-icon {
		width: 50rpx;
		height: 50rpx;
		border-radius: 50rpx;
		text-align: center;
		margin-right: 25rpx;
		background-color: #f3e7e7;
	}

	.content-txt {
		flex: 1;
	}

	/* 退出登录 */
	.Close-login {
		height: 100rpx;
		margin-top: 25rpx;
		margin-bottom: 30rpx;
		border-radius: 20rpx;
		color: #ff4d4f;
		align-items: center;
		justify-content: center;
		background-color: #f1f5f9;
	}

	.Close-login .uni-icons {
		margin-right: 15rpx;
	}

	.function {
		margin-top: 50rpx;
	}

	/* 全局样式 */
	.flex {
		display: flex;
	}
</style>