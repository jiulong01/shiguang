<template>
	<view class="notification-settings-page">
		<!-- 顶部导航栏 -->
		<nav class="top-nav">
			<view class="back-btn" @click="goBack"><uni-icons type="left" size="50rpx"></uni-icons>
			</view>
			<text class="page-title">通知设置</text>
			<view class="settings-icon"><i class="fas fa-cog"></i></view>
		</nav>
		<!-- 通知总开关 -->
		<view class="setting-item-top">
			<view class="icon-wrapper" style="background-color: rgba(249, 115, 22, 0.2);"><i
					class="iconfont icon-tishi"></i></view>
			<span class="setting-name">系统通知</span>
			<switch color="#F97316" :checked="isChecked" @change="switchChange" style="transform:scale(0.7)" />
		</view>
		<!-- 主要内容区域 -->
		<view class="content" v-if="isChecked===true">
			<!-- 通知类型 -->
			<view class="settings-section">
				<text class="section-title">通知类型</text>
				<ul class="settings-list">
					<li class="setting-item">
						<view class="icon-wrapper" style="background-color: rgba(249, 115, 22, 0.2);"><i
								class="iconfont icon-fenlei"></i></view>
						<span class="setting-name">系统通知</span>
						<switch color="#F97316" checked style="transform:scale(0.7)" />
					</li>
					<li class="setting-item">
						<view class="icon-wrapper-green" style="background-color: rgba(76, 175, 80, 0.2);"><i
								class="iconfont icon-naozhong"></i></view>
						<span class="setting-name">任务提醒</span>
						<switch color="#F97316" checked style="transform:scale(0.7)" />
					</li>
					<li class="setting-item">
						<view class="icon-wrapper-blue" style="background-color: rgba(66, 165, 245, 0.2);"><i
								class="iconfont icon-huiyiricheng"></i></view>
						<span class="setting-name">日程提醒</span>
						<switch color="#F97316" checked style="transform:scale(0.7)" />
					</li>
					<li class="setting-item">
						<view class="icon-wrapper-purple" style="background-color: rgba(171, 71, 188, 0.2);"><i
								class="iconfont icon-youjian"></i></view>
						<span class="setting-name">活动通知</span>
						<switch color="#F97316" checked style="transform:scale(0.7)" />
					</li>
				</ul>
			</view>

			<!-- 通知方式 -->
			<view class="settings-section">
				<text class="section-title">通知方式</text>
				<ul class="settings-list">
					<li class="setting-item">
						<view class="icon-wrapper" style="background-color: rgba(249, 115, 22, 0.2);"><i
								class="iconfont icon-shengyin"></i></view>
						<span class="setting-name">声音</span>
						<switch color="#F97316" checked style="transform:scale(0.7)" />
					</li>
					<li class="setting-item">
						<view class="icon-wrapper-green" style="background-color: rgba(76, 175, 80, 0.2);"><i
								class="iconfont icon-zhendong"></i></view>
						<span class="setting-name">振动</span>
						<switch color="#F97316" style="transform:scale(0.7)" />
					</li>
					<li class="setting-item">
						<view class="icon-wrapper-blue" style="background-color: rgba(66, 165, 245, 0.2);"><i
								class="iconfont icon-huojian"></i></view>
						<span class="setting-name">横幅通知</span>
						<switch color="#F97316" checked style="transform:scale(0.7)" />
					</li>
					<li class="setting-item">
						<view class="icon-wrapper-purple" style="background-color: rgba(171, 71, 188, 0.2);"><i
								class="iconfont icon-shandian"></i></view>
						<span class="setting-name">通知灯提示</span>
						<switch color="#F97316" checked style="transform:scale(0.7)" />
					</li>
				</ul>
			</view>

			<!-- 免打扰时段 -->
			<view class="settings-section">
				<text class="section-title">免打扰时段</text>
				<ul class="settings-list">
					<li class="setting-item clickable" @click="openTimeRangeSetting">
						<view class="icon-wrapper" style="background-color: rgba(249, 115, 22, 0.2);"><i
								class="iconfont icon-moonyueliang"></i></view>
						<span class="setting-name">时间段设置</span>
						<text @click="isShow"> {{ settings.doNotDisturb.timeRange }}</text>


						<!-- <span class="time-range"></span> -->
					</li>
					<li class="setting-item">
						<view class="icon-wrapper-green" style="background-color: rgba(76, 175, 80, 0.2);"><i
								class="iconfont icon-yueliang"></i></view>
						<span class="setting-name">每日免打扰</span>
						<switch color="#F97316" style="transform:scale(0.7)" />
					</li>
					<li class="setting-item">
						<view class="icon-wrapper-blue" style="background-color: rgba(66, 165, 245, 0.2);"><i
								class="iconfont icon-laba"></i></view>
						<span class="setting-name">周末免打扰</span>
						<switch color="#F97316" style="transform:scale(0.7)" />
					</li>
				</ul>
			</view>
		</view>
		<view style="height: 30rpx;">

		</view>
		<!-- 隐藏弹窗 -->
		<uni-popup ref="popup" background-color="#fff">
			<yun-picker-time-range @cancle="cancle" @confirm="confirm"></yun-picker-time-range>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		name: 'yun-select-time-range',
		props: {},
		emits: ['cancle', 'confirm'],
		components: {

		},
		data() {
			return {

				settings: {
					notificationTypes: {
						system: true,
						task: true,
						schedule: true,
						event: true
					},
					notificationMethods: {
						sound: true,
						vibration: true,
						banner: true
					},
					doNotDisturb: {
						timeRange: '20:00 - 07:00',
						daily: true,
						weekend: true
					}
				},
				bottomNav: {
					active: 'settings' // 当前激活的导航项
				},
				// 是否打开通知
				isChecked: true,

			};
		},
		methods: {
			switchChange(e) {
				// console.log('switch1 发生 change 事件，携带值为', e.detail.value)
				this.isChecked = e.detail.value
			},
			// 是否展示弹窗
			isShow() {
				this.$refs.popup.open('bottom');
			},
			// 关闭
			close() {
				this.$refs.popup.close();
			},
			// 取消
			cancle() {
				this.close();
				this.$emit('cancle');
			},
			// 确认
			confirm(e) {
				this.$emit('confirm', e);
				this.close();
				const time = e.start_time + " - " + e.end_time
				this.settings.doNotDisturb.timeRange = time
			},
			goBack() {
				// 返回上一页的逻辑
				this.$router.go(-1);
			},
			openTimeRangeSetting() {
				// 打开时间段设置的逻辑

			},

		},
		openTime() {
			this.$refs.timeslot.open()
		}
	};
</script>

<style scoped lang="less">
	@import url("//at.alicdn.com/t/c/font_4987890_j2hhtgx6xe.css");

	/* 基础样式 */
	.notification-settings-page {
		min-height: 100vh;
		font-family: 'Arial', sans-serif;
	}

	/* 状态栏 */
	.status-bar {
		height: 20px;
		padding: 5px 15px;
		display: flex;
		justify-content: space-between;
		font-size: 12px;
		color: #333;
		background-color: white;
	}

	.status-icons {
		display: flex;
		gap: 5px;
	}

	/* 顶部导航栏 */
	.top-nav {
		margin-top: 40rpx;
		height: 50px;
		background-color: white;
		display: flex;
		align-items: center;
		padding: 0 15px;
	}

	.back-btn {
		display: flex;
		align-items: center;
		justify-content: center;
		color: #333;
		font-size: 20px;
	}

	.page-title {
		flex: 1;
		/* text-align: center; */
		font-size: 18px;
		font-weight: 500;
		color: #333;
	}

	.settings-icon {
		width: 40px;
		height: 40px;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #333;
		font-size: 20px;
	}

	/* 内容区域 */

	/* 设置区域通用样式 */
	.settings-section {
		background-color: white;
		border-radius: 15px;
		margin-bottom: 15px;
		overflow: hidden;
	}

	.section-title {
		font-size: 16px;
		color: #666;
		padding: 15px 20px;
		font-weight: 500;
	}

	.settings-list {
		list-style: none;
		padding-left: 0rpx;
		border-radius: 20rpx;
		background-color: #f1f5f9;
		margin: 30rpx;
	}

	.setting-item {
		display: flex;
		align-items: center;
		padding: 15px 20px;

		&-top {
			margin: 10rpx 28rpx;
			display: flex;
			align-items: center;
			padding: 15px 20px;
			background-color: #f1f5f9;
			border-radius: 20rpx;
		}
	}

	.setting-item:last-child {
		border-bottom: none;
	}

	.icon-wrapper {
		width: 30px;
		height: 30px;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #FF7A00;
		margin-right: 15px;
		font-size: 20px;
		background-color: rgba(0, 0, 0, 0.3);
		border-radius: 100rpx;

		&-purple {
			width: 30px;
			height: 30px;
			display: flex;
			align-items: center;
			justify-content: center;
			color: #AB47BC;
			margin-right: 15px;
			font-size: 20px;
			background-color: rgba(0, 0, 0, 0.3);
			border-radius: 100rpx;
		}

		&-green {
			width: 30px;
			height: 30px;
			display: flex;
			align-items: center;
			justify-content: center;
			color: #4CAF50;
			margin-right: 15px;
			font-size: 20px;
			background-color: rgba(0, 0, 0, 0.3);
			border-radius: 100rpx;
		}

		&-blue {
			width: 30px;
			height: 30px;
			display: flex;
			align-items: center;
			justify-content: center;
			color: #42A5F5;
			margin-right: 15px;
			font-size: 20px;
			background-color: rgba(0, 0, 0, 0.3);
			border-radius: 100rpx;
		}
	}

	.setting-name {
		flex: 1;
		font-size: 15px;
		color: #333;
		font-weight: 500;
	}



	/* 免打扰时段特殊样式 */
	.time-range {
		margin-right: 10px;
		color: #999;
		font-size: 14px;
	}

	.arrow-icon {
		color: #999;
		font-size: 16px;
	}

	.clickable {
		cursor: pointer;
	}
</style>