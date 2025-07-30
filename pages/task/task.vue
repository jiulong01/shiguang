<template>
	<view class="task-app">

		<!-- 顶部问候信息 -->
		<view class="greeting">
			<view class="greeting-left">
				<text class="greeting-text">{{greeting}}，Alex</text>
				<text class="date">{{currentDate}} {{currentDay}}</text>

			</view>
			<view class="greeting-avatar">
				<img src="@/static/task/task-avatar.png" class="greeting-avatar-img" alt="" />
				<view class="greeting-avatar-premium">
					<img src="@/static/vip/vipLogo.png" class="greeting-avatar-premium-img" alt="" />
				</view>
			</view>

		</view>

		<!-- 日期导航 -->
		<view class=" date-nav">
			<DateSelector></DateSelector>
		</view>

		<!-- 任务统计 -->
		<view class="task-stats">
			<span class="stat-item">
				<p class="stat-item-number stat-item-number-one">5</p>
				<p class="stat-item-tip">今日待办</p>
			</span>
			<span class="stat-item">
				<p class="stat-item-number stat-item-number-two">3</p>
				<p class="stat-item-tip">已完成</p>
			</span>
			<span class="stat-item ">
				<p class="stat-item-number stat-item-number-three">2</p>
				<p class="stat-item-tip">重要事件</p>
			</span>
			<span class="stat-item">
				<p class="stat-item-number stat-item-number-four">2</p>
				<p class="stat-item-tip">逾期事件</p>
			</span>
		</view>

		<!-- 分类列表 -->
		<view class="task-actions">
			<view class="task-actions-tip">
				<i class="iconfont icon-icon custom-icon"></i>
				<p class="task-actions-tip-p">所有任务</p>
			</view>
			<view class="task-actions-tip">
				<i class="iconfont icon-fenlei custom-icon"></i>
				<p class="task-actions-tip-p">分类</p>
			</view>
			<view class="task-actions-tip">
				<i class="iconfont icon-naozhong custom-icon"></i>
				<p class="task-actions-tip-p">时间</p>
			</view>
			<view class="task-actions-tip">
				<i class="iconfont icon-xingxing custom-icon"></i>
				<p class="task-actions-tip-p">紧急程度</p>
			</view>
		</view>

		<!-- 高级会员推广 -->
		<Member></Member>

		<!-- 任务列表 -->
		<view class="task-list">
			<view v-for="(task, index) in tasks" :key="index" class="task-item" :class="getTaskClass(task.category)">
				<view class="task-content">
					<h3 :style="getTextStyle(task)">{{ task.title }}</h3>
					<view class="task-content-foot">
						<p v-if="task.time" class="task-content-foot-p" :style="getTextStyle(task)">
							<i class="iconfont icon-shijian"></i>
							{{ task.time }}
						</p>
						<p v-if="task.repeat" class="task-content-foot-p" :style="getTextStyle(task)">
							<i class="iconfont icon-rili"></i>
							{{ task.repeat }}
						</p>
						<p v-if="task.status === '逾期'" class="task-content-foot-p overdue" :style="getTextStyle(task)">
							{{ task.status }}
						</p>
					</view>
				</view>
				<view class="task-checkbox" :class="{ 'completed': task.completed }" @click="toggleComplete(index)">
					<i class="iconfont icon-duigou task-checkbox-icon" :class="{ 'completed': task.completed }"></i>
				</view>
			</view>
		</view>

		<!-- 补位盒子 -->
		<view class="lastTab"></view>
		<!-- 底部新建任务按钮 -->
		<BottomBar></BottomBar>
	</view>
</template>

<script>
	import calendar from '../../components/calendar/calendar.vue';
	import Member from '../../components/Member/Member.vue';
	import BottomBar from "../../components/BottomBar/BottomBar.vue"
	export default {
		components: {
			calendar
		},
		data() {
			return {
				// 标头时间实时更新数据
				currentTime: new Date(),
				timer: null,
				// 假数据
				days: [{
						date: '5',
						day: '周四'
					},
					{
						date: '6',
						day: '周五'
					},
					{
						date: '7',
						day: '周六'
					},
					{
						date: '8',
						day: '周日'
					},
					{
						date: '9',
						day: '周一'
					},
					{
						date: '10',
						day: '周二'
					},
					{
						date: '11',
						day: '周三'
					}
				],
				tasks: [{
						title: '完成产品设计方案',
						important: true,
						time: '今天 14:00',
						repeat: '每周一、三、五',
						completed: false,
						status: '',
						category: '工作'
					},
					{
						title: '健身房锻炼',
						time: '今天 18:30',
						repeat: '每天',
						completed: false,
						status: '',
						category: '健康'
					},
					{
						title: '购买生日礼物',
						time: '明天 12:00',
						repeat: '',
						completed: false,
						status: '',
						category: '生活'
					},
					{
						title: '阅读《原子习惯》第三章',
						time: '昨天 20:00',
						repeat: '',
						completed: false,
						status: '逾期',
						category: '学习'
					},
					{
						title: '回复工作邮件',
						time: '昨天 17:00',
						repeat: '',
						completed: true,
						status: '',
						category: '工作'
					}
				]
			};
		},
		methods: {
			// 根据category获取对应的类名
			getTaskClass(category) {
				switch (category) {
					case '生活':
						return 'category-life';
					case '健康':
						return 'category-health';
					case '学习':
						return 'category-study';
					case '工作':
						return 'category-work';
					case '家庭':
						return 'category-family';
					case '其他':
						return 'category-other';
					default:
						return '';
				}
			},
			// 逾期则颜色改变
			getTextStyle(task) {
				if (task.completed || task.status === '逾期') {
					return {
						color: '#999999',
						textDecoration: 'line-through'
					};
				}
				return {};
			},
			// 新增方法：切换完成状态
			toggleComplete(index) {
				this.tasks[index].completed = !this.tasks[index].completed;
				// 强制更新视图
				this.$forceUpdate();
			},
		},
		computed: {
			// 标头时间实时更新操作	
			// 当前日期格式化
			currentDate() {
				const year = this.currentTime.getFullYear();
				const month = this.currentTime.getMonth() + 1;
				const date = this.currentTime.getDate();
				return `${year}年${month}月${date}日`;
			},
			// 当前星期
			currentDay() {
				const days = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
				return days[this.currentTime.getDay()];
			},
			// 动态问候语
			greeting() {
				const hours = this.currentTime.getHours();
				if (hours >= 9 && hours < 11) {
					return '上午好';
				} else if (hours >= 11 && hours < 14) {
					return '中午好';
				} else if (hours >= 14 && hours < 19) {
					return '下午好';
				} else if (hours >= 19 && hours < 23) {
					return '晚上好';
				} else {
					return '早上好';
				}
			},
			mounted() {
				// 每分钟更新一次时间
				this.timer = setInterval(() => {
					this.currentTime = new Date();
				}, 60000);
			},
			beforeDestroy() {
				// 组件销毁前清除定时器
				if (this.timer) {
					clearInterval(this.timer);
				}
			}
			//
		},
	};
</script>

<style scoped lang="less">
	@import url("https://at.alicdn.com/t/c/font_4947529_2mhc2oxx1bq.css");
	@import url("https://at.alicdn.com/t/c/font_4945078_d8s44zrrq5.css");

	/* 整体样式 */
	.task-app {
		height: 100%;
		padding: 20px;
		margin-top: 50rpx;
		font-family: Arial, sans-serif;
	}

	/* 标头用户样式 */
	.greeting {
		margin-bottom: 30rpx;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		align-items: center;

		&-text {
			font-size: 20px;
			font-weight: bold;
			color: #333;
			// margin-right: 60rpx;
		}

		&-left {
			display: flex;
			flex-direction: column;
		}
	}

	.date {
		font-size: 14px;
		color: #999;
		margin: 10rpx 0;
		width: 100%;
	}

	.greeting-avatar {
		width: 15%;
		height: 15%;
		border-radius: 50%;
		position: relative;
	}

	.greeting-avatar-img {
		border-radius: 100%;
		width: 100%;
		height: 100%;
	}

	.greeting-avatar-premium {
		width: 40rpx;
		height: 40rpx;
		background-color: #fff;
		border-radius: 100%;
		text-align: right;
		position: absolute;
		top: 0rpx;
		right: 0rpx;

		&-img {
			width: 40rpx;
			height: 27rpx;
			position: absolute;
			top: 3rpx;
			right: -5rpx;
		}
	}

	/* 日期导航样式 */
	.date-nav {
		width: 100%;
		margin-bottom: 20px;
	}

	/* 任务统计样式 */
	.task-stats {
		display: flex;
		justify-content: space-around;
		align-items: center;
		margin-bottom: 20px;
	}

	.stat-item {
		width: 22%;
		height: 120rpx;
		text-align: center;
		background-color: #F1F5F9;
		border-radius: 10rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.stat-item-number {
		font-size: 40rpx;
	}

	.stat-item-tip {
		font-size: 20rpx;
	}

	.stat-item-number-one {
		color: #F97316;
	}

	.stat-item-number-two {
		color: #22C55E;
	}

	.stat-item-number-three {
		color: #EF4444;
	}

	.stat-item-number-four {
		color: #666666;
	}

	/* 分类列表样式 */
	.task-actions {
		width: 100%;
		margin-bottom: 20px;
		display: flex;
		justify-content: space-around;
		align-items: center;
	}

	.task-actions-tip {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.custom-icon {
		font-size: 40rpx;
		margin-bottom: 10rpx;
	}

	.task-actions-tip-p {
		font-size: 20rpx;
	}



	/* 任务列表样式 */
	.task-list {
		margin-bottom: 20px;
	}

	.task-item {
		background-color: #F1F5F9;
		margin-bottom: 20rpx;
		border-radius: 15rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 10px;
		border-left: 15rpx solid red;
	}

	.task-content {
		flex: 1;
	}

	.task-content-foot {
		display: flex;
	}

	.task-content-foot-p {
		color: #666666;
		margin: 10rpx;
	}

	.overdue {
		color: red;
	}

	.task-checkbox {
		width: 40rpx;
		height: 40rpx;
		border: 5rpx solid black;
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		transition: all 0.3s ease;
	}

	.task-checkbox.completed {
		border: 5rpx solid #F97316;
		background-color: #F97316;
	}

	.task-checkbox-icon {
		font-size: 40rpx;
		font-weight: bold;
	}

	.task-checkbox-icon.completed {
		color: white;
		/* 完成时显示白色 */
	}

	.task-item.category-life {
		border-left: 15rpx solid #4CAF50;
	}

	.task-item.category-health {
		border-left: 15rpx solid #2196F3;
	}

	.task-item.category-study {
		border-left: 15rpx solid #FF9800;
	}

	.task-item.category-work {
		border-left: 15rpx solid #9C27B0;
	}

	.task-item.category-family {
		border-left: 15rpx solid #F44336;
	}

	.task-item.category-other {
		border-left: 15rpx solid #9E9E9E;
	}

	.lastTab {
		height: 100rpx;
	}

	/* 底部样式 */
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