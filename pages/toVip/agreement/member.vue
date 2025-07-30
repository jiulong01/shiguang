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
			<view class="header-content">
				<view class="content-title">
					拾光VIP会员服务协议
				<view class="title-underline"></view>
			</view>
		</view>
	</view>
		
		<!-- 可滚动内容区 -->
		<scroll-view class="scroll-content" scroll-y>
			<view class="content-good">
				<!-- 一、会员服务内容 -->
				<span class="section-title">一、会员服务内容</span>
				<view class="indent-text">开通拾光VIP会员后，您可享受以下权益：</view>
				<view class="indent-item">1、无限量任务提醒设置；</view>
				<view class="indent-item">2、高级自定义提醒音效与主题；</view>
				<view class="indent-item">3、多设备同步功能；</view>
				<view class="indent-item">4、专属客服优先响应</view>
				
				<!-- 二、会员类型与价格 -->
				<span class="section-title">二、会员类型与价格</span>
				<view class="indent-text">1、会员类型：</view>
				<view class="indent-item highlight">月度会员：¥18/月</view>
				<view class="indent-item highlight">年度会员：¥98/年</view>
				<view class="indent-item highlight">终身会员：￥298（一次付费，终身使用）</view>
				<view class="indent-text">2、价格可能因促销活动调整，实际支付金额以订单页为准。</view>
				
				<!-- 三、订阅与取消 -->
				<span class="section-title">三、订阅与取消</span>
				<view class="indent-text">1、订阅后，会员有效期自动续期，直至用户主动取消。</view>
				<view class="indent-text">2、取消路径：</view>
				<view class="indent-item platform">iOS用户：前往Apple ID设置管理订阅</view>
				<view class="indent-item platform">Android用户：通过Google Play订阅中心操作</view>
				
				<!-- 四、退款政策 -->
				<span class="section-title">四、退款政策</span>
				<view class="indent-text">1、会员服务为虚拟商品，开通后不支持无理由退款。</view>
				<view class="indent-text">2、因服务故障导致无法使用，可联系客服申请补偿。</view>
				
				<!-- 五、用户义务 -->
				<span class="section-title">五、用户义务</span>
				<view class="indent-text important">禁止将会员账号用于商业用途或违反法律法规的行为。</view>
				
				<!-- 联系信息 -->
				<view class="contact-container">
					<view class="indent-text contact">联系客服：support@shiguangapp.com</view>
					<view class="content-time">生效日期：{{ dynamicDate }}</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			dynamicDate: ''
		}
	},
	mounted() {
		this.updateDate()
	},
	methods: {
		updateDate() {
		  const now = new Date();
		  const year = now.getFullYear();
		  const month = now.getMonth() + 1;
		  const day = now.getDate();
		  this.dynamicDate = `${year}年${month}月${day}日`;
		},
		back() {
			uni.navigateBack()
			console.log('返回上一页');
			
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
		z-index: 100;
		background-color: #fff9f6;
		padding: 20rpx 0;
		box-shadow: 0 2rpx 10rpx rgba(234, 88, 12, 0.1);
		padding-top: 88rpx;
		
		.header-content {
			max-width: 60%;
			margin: 0 auto;
		}
		
		.content-title {
			width: 81%;
			text-align: center;
			font-weight: bold;
			font-size: 36rpx;
			color: #EA580C;
			letter-spacing: 1rpx;
			position: relative;
			margin: 0 auto;
		}
		
		.title-underline {
			width: 140rpx;
			height: 6rpx;
			background: linear-gradient(to right, #EA580C, #ffb800);
			margin: 12rpx auto 0;
			border-radius: 3rpx;
		}
	}
	
	.scroll-content {
		flex: 1;
		overflow-y: auto;
		
		.content-good {
			max-width: 90%;
			margin: 0 auto;
			padding: 20rpx 0 40rpx;
			
			.section-title {
				font-weight: bold;
				font-size: 34rpx;
				margin: 40rpx 0 20rpx;
				color: #EA580C;
				display: block;
				border-left: 6rpx solid #EA580C;
				padding-left: 15rpx;
				background-color: rgba(234, 88, 12, 0.05);
			}
			
			.indent-text {
				margin: 16rpx 0;
				color: #333;
				font-size: 30rpx;
				
				&.important {
					color: #d32f2f;
					font-weight: bold;
					border: 1rpx solid #ffcdd2;
					padding: 10rpx;
					background-color: #ffebee;
				}
				
				&.contact {
					color: #EA580C;
					font-size: 28rpx;
				}
			}
			
			.indent-item {
				margin: 12rpx 0;
				padding-left: 40rpx;
				color: #555;
				font-size: 29rpx;
				
				&.highlight {
					color: #EA580C;
					font-weight: 500;
				}
				
				&.platform {
					color: #5c6bc0;
					font-size: 28rpx;
					background-color: #f5f7ff;
					padding: 12rpx 20rpx;
					border-radius: 8rpx;
					border-left: 4rpx solid #5c6bc0;
				}
			}
			
			.contact-container {
				margin-top: 40rpx;
				position: relative;
				padding-bottom: 50rpx;
				border-top: 1rpx dashed #EA580C;
				padding-top: 20rpx;
				
				.content-time {
					position: absolute;
					right: 0;
					bottom: 0;
					font-size: 26rpx;
					color: #EA580C;
					opacity: 0.8;
				}
			}
		}
	}
</style>
