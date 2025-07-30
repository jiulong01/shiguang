<template>
	<view class="container">
		<!-- 固定标题栏 -->
		<view class="fixed-header">
			<uni-icons 
				type="back" 
				size="30" 
				style="position: absolute; width: 10%; left: 20rpx; color: #EA580C;"
				@click="back">
			</uni-icons>
			<view class="content-title">
				自动续费规则
			</view>
		</view>
		
		<!-- 可滚动内容区 -->
		<scroll-view class="scroll-content" scroll-y>
			<view class="content-auto">
				<span class="section-title">一、自动续费规则</span>
				<view class="indent-text">1、订阅会员时，默认启用自动续费功能：</view>
				<view class="indent-item">月度会员：每30天自动扣费</view>
				<view class="indent-item">年度会员：每365天自动扣费</view>
				<view class="indent-text">2、扣费前3天将通过App推送或短信提醒</view>
				
				<span class="section-title">二、扣费方式</span>
				<view class="indent-text">1、费用从您绑定的支付账户（Apple Pay/Google Pay/支付宝/微信支付）自动扣除。</view>
				<view class="indent-text">2、若扣费失败，会员权益将在到期后暂停。</view>
				
				<span class="section-title">三、如何关闭自动续费</span>
				<view class="indent-text">1、关闭后，会员权益保留至当前周期结束。</view>
				<view class="indent-text">2、关闭路径：</view>
				<view class="indent-item platform">iOS：设置 → Apple ID → 订阅 → 拾光 → 取消订阅</view>
				<view class="indent-item platform">Android：Google Play → 订阅 → 拾光 → 取消</view>
				
				<span class="section-title">四、注意事项</span>
				<view class="indent-text">1、自动续费由苹果/谷歌支付系统处理，拾光无法直接干预扣费流程。</view>
				<view class="indent-text">2、若卸载App，仍需通过上述路径取消订阅以避免扣费。</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			back() {
				uni.navigateBack()
			}
		}
	}
</script>

<style lang="less" scoped>
	.container {
		height: 100vh;
		display: flex;
		flex-direction: column;
		background-color: #fff9f6;
	}
	
	.fixed-header {
		position: sticky;
		top: 0;
		z-index: 10;
		background-color: #fff9f6;
		padding: 20rpx 0;
		box-shadow: 0 2rpx 10rpx rgba(234, 88, 12, 0.1);
		width: 100%;
		padding-top: 88rpx;
		.content-title {
			text-align: center;
			font-weight: bold;
			font-size: 38rpx;
			color: #EA580C;
			position: relative;
			max-width: 80%;
			margin: 0 auto;
			&::after {
				content: "";
				display: block;
				width: 120rpx;
				height: 6rpx;
				background: linear-gradient(to right, #EA580C, #ffb800);
				margin: 10rpx auto 0;
				border-radius: 3rpx;
			}
		}
	}
	
	.scroll-content {
		flex: 1;
		overflow-y: auto;
		
		.content-auto {
			max-width: 90%;
			margin: 0 auto;
			padding-top: 20rpx;
			padding-bottom: 40rpx;
			
			/* 其他样式保持不变 */
			.section-title {
				font-weight: bold;
				font-size: 34rpx;
				margin: 40rpx 0 20rpx;
				color: #EA580C;
				display: block;
				border-left: 8rpx solid #EA580C;
				padding-left: 20rpx;
				background-color: rgba(234, 88, 12, 0.05);
			}
			
			.indent-text {
				margin: 16rpx 0;
				padding-left: 20rpx;
				position: relative;
			}
			
			.indent-item {
				margin: 12rpx 0;
				padding-left: 60rpx;
				position: relative;
				color: #555;
				
				&.platform {
					color: #5c6bc0;
					font-size: 28rpx;
					background-color: #f5f7ff;
					padding: 12rpx 20rpx;
					border-radius: 8rpx;
					border-left: 4rpx solid #5c6bc0;
				}
			}
		}
	}
</style>