<template>
	<view class="premium-member">
		<view class="premium-alert">
			<view class="premium-content">
				<view class="premium-content-title">
					<view class="premium-content-left">
						<img src="@/static/vip/vipLogo.png" class="premium-img" alt="" />
						<span class="premium-text">高级会员</span>
					</view>
					<view class="premium-button" @click="toVip()">立即开通</view>
				</view>
				<span class="premium-desc">解锁四象限管理、数据统计、云同步等高级功能</span>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		methods: {
			toVip() {
				uni.navigateTo({
					url: '/pages/toVip/toVip',
				});
			},
		},
	}
</script>

<style lang="less">
	.premium-member {
		margin-bottom: 20px;
		position: relative;
	}

	.premium-alert {
		background: linear-gradient(to right, #EA580C, #FB923C);
		border: none;

		align-items: center;
		padding: 10px 15px;
		border-radius: 30rpx;
	}

	.premium-content {
		display: flex;
		flex-direction: column;
		margin-bottom: 10rpx;

		&-left {
			margin-bottom: 15rpx;
		}
	}

	.premium-content-title {
		margin-bottom: 10rpx;
		display: flex;
		justify-content: space-between;
	}

	.premium-img {
		margin-right: 5px;
	}

	.premium-text {
		color: white;
		font-size: 30rpx;
		margin-right: 5px;
	}

	.premium-desc {
		font-size: 25rpx;
		color: white;
	}

	.premium-button {
		width: 130rpx;
		height: 50rpx;
		text-align: center;
		line-height: 50rpx;
		background: linear-gradient(to right, rgba(255, 255, 255, 0.5), rgba(255, 255, 255, 0.2));
		border-radius: 30rpx;
		color: white;
		border: none;
		/* position: absolute; */
		top: 20rpx;
		right: 60rpx;
		font-size: 20rpx;
	}

	.premium-button:hover {
		background-color: #f5f5f5;
	}
</style>