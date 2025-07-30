<template>
	<view class="new-task-container">
		<view class="new-task-icon">
			+
		</view>
		<view class="new-task-btn" @click="toCreateTask()">
			<span class="new-task-text">新建任务</span>
		</view>
		<view class="new-task-icon-right">
			<i class="iconfont icon-michuatong"></i>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			// 跳转到创建任务页面
			toCreateTask() {
				uni.navigateTo({
					url: '/pages/task/createTask/createTask'
				});
			},
		}
	}
</script>

<style scoped lang="less">
	.new-task-container {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 5px 0;
		position: fixed;
		bottom: 100rpx;

		right: 0;
		background-color: #FFFFFF;
		z-index: 999;
	}

	.new-task-btn {
		flex: 1;
		background: linear-gradient(to right, #FF6347, #FFA500);
		border: none;
		border-radius: 20px;
		padding: 12px 40px;
		margin: 0 10px;
		display: inline-flex;
		align-items: center;
		justify-content: center;
		transition: all 0.3s ease;
		/* 添加过渡效果 */
	}


	.new-task-icon {
		width: 80rpx;
		height: 80rpx;
		background-color: #eee;
		text-align: center;
		line-height: 80rpx;
		border-radius: 50%;
		font-size: 48rpx;
		margin-left: 10rpx;

		&-right {
			width: 80rpx;
			height: 80rpx;
			background-color: #eee;
			text-align: center;
			line-height: 80rpx;
			border-radius: 50%;
			font-size: 48rpx;
			margin-right: 10rpx;
		}

	}

	.new-task-text {
		font-size: 14px;
		color: white;
	}

	.new-task-mic-icon {
		font-size: 30px;
		color: #FFA500;
		/* 与按钮颜色呼应 */
		background-color: #F1F5F9;
		border-radius: 50%;
		margin: 20rpx;
	}
</style>