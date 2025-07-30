<template>
	<view class="vipLayout">
		<view class="" style="height: 50rpx;"></view>
		<view :style="{
			position: 'fixed', 
			top: '0rpx', 
			width: '100%', 
			zIndex: 99 , backgroundColor: 'white' , marginBottom: '80rpx' , paddingTop: statusBarHeight }">
			<view class="" style="height: 60rpx;"></view>
			<view class="topbar">
				<uni-icons type="back" size="30"
					style="color: black; width: 10%; text-align: center; margin-left: 20rpx;" @click="back"></uni-icons>
				<h4 style="text-align:center; line-height: 90rpx;">高级会员</h4>
			</view>
		</view>
		<view class="premium-member-container" :style="{paddingTop: statusBarHeight}">

			<!-- 会员头部 -->

			<image src="@/static/vip/background.png" class="background-image" mode="scaleToFill" />
			<!-- 会员权益 -->
			<view class="member-benefits">
				<text class="benefits-title">会员权益</text>
				<view class="benefits-list">
					<view class="benefit-item" v-for="(item, index) in benefits" :key="index">
						<view class="benefit-icon">
							<image :src="item.icon" style="width: 55% ; height: 55%"></image>
						</view>
						<text class="benefit-name">{{ item.name }}</text>
						<text class="benefit-desc">{{ item.desc }}</text>
					</view>
				</view>
			</view>
			<!-- 价格方案 -->
			<view class="price-plans">
				<text class="plans-title">价格方案</text>
				<view class="plan-item" :class="{ active: selectedPlan === plan.type }" v-for="(plan, index) in plans"
					:key="index" @click="selectedPlan = plan.type">
					<view>
						<text class="plan-type">
							{{ plan.name }}
							<text class="tag" v-if="plan.tag">{{ plan.tag }}</text>
							<text class="tag popular" v-if="plan.popular">最受欢迎</text>
						</text>
						<text class="plan-price">{{ plan.price }}</text>
						<text class="plan-desc">{{ plan.desc }}</text>
					</view>
					<uni-icons :type="selectedPlan === plan.type ? 'circle-filled' : 'circle'"
						:color="selectedPlan === plan.type ? '#ff9933' : '#999'" size="20"
						class="checkbox-icon"></uni-icons>
				</view>
			</view>
			<!-- 支付方式 -->
			<view class="payment-methods">
				<text class="methods-title">支付方式</text>
				<view class="methods-list">
					<view class="method-item" :class="{ active: selectedPayment === method.type }"
						v-for="(method, index) in paymentMethods" :key="index" @click="selectedPayment = method.type">
						<image :src="method.icon" class="method-icon" mode="aspectFit"></image>
						<text>{{ method.name }}</text>
					</view>
				</view>
			</view>
			<!-- 立即开通按钮及协议 -->
			<button class="btn-open" @click="handleOpen">立即开通</button>
			<!-- 协议条款 -->
			<view class="agreement-container">
				<uni-icons :type="agreeToTerms ? 'checkbox-filled' : 'circle'"
					:color="agreeToTerms ? '#ff9933' : '#999'" size="20"
					@click="agreeToTerms = !agreeToTerms"></uni-icons>
				<text class="agreement">
					开通即表示您已同意
					<text class="link" @click="showAgreement('member')">《会员服务协议》</text>
					和
					<text class="link" @click="showAgreement('autoRenew')">《自动续费协议》</text>
				</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				selectedPlan: 'year', // 默认选中年度会员
				selectedPayment: 'alipay', // 默认选中支付宝
				agreeToTerms: false, // 新增协议勾选状态
				statusBarHeight: 0, // 状态栏高度
				benefits: [{
						name: '四象限管理',
						icon: '/static/vip/four.png',
						desc: '高效任务分类与优先级管理'
					},
					{
						name: '云同步',
						icon: '/static/vip/cloud.png',
						desc: '多设备数据实时同步'
					},
					{
						name: '数据分析',
						icon: '/static/vip/data.png',
						desc: '任务完成率与效率分析'
					},
					{
						name: '无广告体验',
						icon: '/static/vip/ad.png',
						desc: '纯净界面，专注效率'
					}
				],
				plans: [{
						type: 'month',
						name: '月度会员',
						price: '¥18 /月',
						desc: '每月自动续费，随时可取消'
					},
					{
						type: 'year',
						name: '年度会员',
						price: '¥98 /年',
						desc: '相当于8.2元/月，节省56%',
						tag: '限时优惠',
						popular: true
					},
					{
						type: 'lifetime',
						name: '终身会员',
						price: '¥298',
						desc: '一次付费，终身使用'
					}
				],
				paymentMethods: [{
						type: 'alipay',
						name: '支付宝',
						icon: '/static/vip/payLogo.png'
					},
					{
						type: 'wechat',
						name: '微信支付',
						icon: '/static/vip/weChatLogo.png'
					},
					{
						type: 'applepay',
						name: 'Apple Pay',
						icon: '/static/vip/iphonePayLogo.png'
					}
				]
			}
		},
		onLoad() {
			this.getStatusBarHeight()
			console.log(this.statusBarHeight);
		},
		methods: {
			// goToPrivilege() {
			// 	// 跳转到高级特权页面，使用 uni-app 路由
			// 	uni.navigateTo({
			// 		url: '/pages/privilege/index'
			// 	});
			// },
			back() {
				// 返回上一页
				uni.navigateBack();
			},
			handleOpen() {
				if (!this.agreeToTerms) {
					uni.showToast({
						title: '请先阅读并同意协议',
						icon: 'none'
					});
					return;
				}
				// 调用支付接口进行会员开通
				console.log('选择的方案：', this.selectedPlan);
				console.log('选择的支付方式：', this.selectedPayment);

				// 模拟支付流程
				uni.showLoading({
					title: '正在处理...'
				});

				// 实际项目中这里应该调用后端支付接口
				setTimeout(() => {
					uni.hideLoading();

					uni.showModal({
						title: '支付成功',
						content: '恭喜您成为拾光高级会员！',
						showCancel: false,
						success: () => {
							// 支付成功后跳转到会员中心或刷新当前页面
							uni.navigateBack();
						}
					});
				}, 1500);
			},
			showAgreement(type) {
				// 显示协议内容
				uni.navigateTo({
					url: `/pages/toVip/agreement/${type}`
				});
			},
			// 获取状态栏高度
			getStatusBarHeight() {
				const systemInfo = uni.getSystemInfoSync()
				this.statusBarHeight = systemInfo.statusBarHeight * 2 + 'rpx'
				uni.setStorageSync('statusBarHeight', this.statusBarHeight)
			},
		}
	}
</script>

<style scoped>
	/* 全局样式 */
	page {
		background-color: #ffffff;
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
		color: #333;
	}

	.topbar {
		width: 100%;
		height: 90rpx;
		background-color: #fff;
		display: flex;


		/* line-height: 10rpx; */
		align-items: center;
	}

	.premium-member-container {
		width: 90%;
		max-width: 500px;
		margin: 0 auto;
		padding: 20rpx;
	}

	/* .member-header {
		background: linear-gradient(to right, #ff9933, #ffb86c);
		border-radius: 20rpx;
		color: #F1F5F9;
		padding: 40rpx;
		margin-bottom: 40rpx;
	} */

	/* .header-content {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
	} */
	.background-image {
		width: 100%;
		height: 300rpx;
		margin-bottom: 20rpx;
		/* margin-top: 80rpx; */
		margin-top: 110rpx;
	}

	.member-title {
		font-size: 36rpx;
		font-weight: bold;
		display: flex;
		align-items: center;
	}

	.icon-crown {
		width: 40rpx;
		height: 40rpx;
		margin-right: 10rpx;
	}

	.member-desc {
		font-size: 28rpx;
		margin: 16rpx 0;
	}

	.btn-enter {
		background-color: #fff;
		color: #ff9933;
		border: none;
		padding: 16rpx 32rpx;
		border-radius: 10rpx;
		font-size: 28rpx;
	}

	.member-benefits {
		margin-bottom: 40rpx;
	}

	.benefits-title {
		font-size: 32rpx;
		font-weight: bold;
		margin-bottom: 20rpx;
		display: block;
	}

	.benefits-list {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 20rpx;
	}

	.benefit-item {
		width: auto;
		margin: 0;
		background-color: #F1F5F9;
		border-radius: 10rpx;
		padding: 20rpx;
		margin-bottom: 20rpx;
		box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
	}

	.benefit-icon {
		width: 60rpx;
		height: 60rpx;
		margin-bottom: 10rpx;
		border-radius: 50%;
		background: linear-gradient(to right bottom, #EA580C, #FB923C);
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.benefit-name {
		font-size: 28rpx;
		font-weight: bold;
		margin-bottom: 10rpx;
		display: block;
	}

	.benefit-desc {
		font-size: 24rpx;
		color: #999;
		display: block;
	}

	.price-plans {
		margin-bottom: 40rpx;
	}

	.plans-title {
		font-size: 32rpx;
		font-weight: bold;
		margin-bottom: 20rpx;
		display: block;
	}

	.plan-item {
		background-color: #F1F5F9;
		border: 4rpx solid #eee;
		border-radius: 20rpx;
		padding: 20rpx;
		margin-bottom: 20rpx;
		cursor: pointer;
		transition: all 0.3s ease;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.plan-item.active {
		border-color: #ff9933;
		background-color: #fff3e6;
		transform: scale(1.01);
		box-shadow: 0 4rpx 16rpx rgba(255, 153, 51, 0.15);
	}

	.plan-type {
		font-size: 28rpx;
		font-weight: 400;
		margin-bottom: 10rpx;
		display: block;
	}

	.tag {
		font-size: 20rpx;
		color: #fff;
		background-color: #ff9933;
		padding: 4rpx 8rpx;
		border-radius: 6rpx;
		margin-left: 10rpx;
	}

	.popular {
		background-color: #ff5722;
	}

	.plan-price {
		font-size: 36rpx;
		color: #ff9933;
		margin-bottom: 10rpx;
		display: block;
		font-weight: 600;
	}

	.plan-desc {
		font-size: 24rpx;
		color: #999;
		display: block;
	}

	.payment-methods {
		margin-bottom: 40rpx;
	}

	.methods-title {
		font-size: 32rpx;
		font-weight: bold;
		margin-bottom: 20rpx;
		display: block;
	}

	.methods-list {
		display: flex;
		justify-content: space-between;
	}

	.method-item {
		width: 30%;
		text-align: center;
		border: 2rpx solid #eee;
		border-radius: 10rpx;
		padding: 20rpx;
		cursor: pointer;
		transition: all 0.3s ease;
		font-size: small;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.method-item.active {
		border-color: #ff9933;
		background-color: #fff3e6;
		transform: scale(1.05);
	}

	.method-icon {
		height: 40rpx;
		width: 45rpx;
		margin-bottom: 10rpx;
	}

	.btn-open {
		width: 100%;
		height: 80rpx;
		background: linear-gradient(to right bottom, #EA580C, #FB923C);
		color: #fff;
		border: none;
		padding: 0;
		border-radius: 40rpx;
		font-size: 32rpx;
		line-height: 80rpx;
		margin-top: 20rpx;
	}

	.agreement-container {
		display: flex;
		align-items: center;
		margin-top: 20rpx;
		padding: 10rpx 0;
	}

	.agreement {
		display: block;
		text-align: center;
		font-size: 24rpx;
		color: #999;
		margin-left: 10rpx;
		padding: 10rpx 0;
	}

	.link {
		color: #ff9933;
		text-decoration: none;
	}
</style>