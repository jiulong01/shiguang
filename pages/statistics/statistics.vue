<template>
	<view class="container">
		<view class="" style="height: 60rpx;"></view>
		<!-- 顶部信息栏 -->
		<view class="header">
			<view class="greeting">
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

		<!-- 日期导航栏 -->
		<DateSelector></DateSelector>

		<!-- 任务统计标题 -->
		<view class="task-stat-title">
			<text>任务统计</text>
			<text class="sub-text">查看您的任务完成情况和效率分析</text>
		</view>

		<!-- 任务完成情况 - 环形图 -->
		<view class="task-card">
			<view class="chart-title">任务完成情况</view>
			<view class="flex">
				<view class="charts-arcbar">
					<qiun-data-charts type="arcbar" :opts="arcbarOpts" :chartData="arcbarData" />
				</view>
				<view class="task-info">
					<text class="week-target">本周目标</text>
					<text class="completion-rate">↑ 完成率+12%</text>
					<text class="completed-task">完成 18 项任务</text>
				</view>
			</view>
		</view>

		<!-- 任务类别分布 - 饼图 -->
		<view class="task-card">
			<view class="chart-title">任务类别分布</view>
			<view class="flex">
				<view class="charts-ring">
					<qiun-data-charts type="ring" :opts="ringOpts" :chartData="ringData" />
				</view>
				<!-- <view class="category-info">
					<view class="category-item" v-for="(item, index) in categoryList" :key="index">
						<view :style="{ backgroundColor: item.color }" class="category-dot"></view>
						<text>{{ item.name }} {{ item.percent }}</text>
					</view>
				</view> -->
			</view>
		</view>

		<!-- 时间利用分析 - 柱状图 -->
		<view class="task-card">
			<view class="chart-title">时间利用分析</view>
			<view class="charts-box">
				<qiun-data-charts type="column" :opts="opts" :chartData="chartData" />
			</view>
			<view class="time-use-info">
				<text>效率最高：周三</text>
				<text>平均每日完成：3.5项</text>
			</view>
		</view>

		<!-- 高级会员横幅 -->
		<Member></Member>
		<view class="" style="height: 100rpx;">

		</view>
		<!-- 新建任务按钮 -->
		<BottomBar></BottomBar>
	</view>
</template>

<script>
	import DateSelector from '@/components/DateSelector/DateSelector.vue';
	import Member from '../../components/Member/Member.vue';
	import BottomBar from "../../components/BottomBar/BottomBar.vue"
	export default {
		components: {
			DateSelector
		},
		data() {
			return {
				// 任务完成情况图表配置
				currentTime: new Date(),
				arcbarData: {},
				arcbarOpts: {
					color: ["#EE6666"],
					padding: undefined,
					title: {
						name: "80%",
						fontSize: 18,
						color: "black"
					},
					subtitle: {
						name: "18/20",
						fontSize: 15,
						color: "#666666"
					},
					extra: {
						arcbar: {
							type: "circle",
							width: 12,
							backgroundColor: "#E9E9E9",
							startAngle: 1.5,
							endAngle: 0.25,
							gap: 2
						}
					}
				},
				// 任务类别分布
				ringData: {},
				ringOpts: {
					rotate: false,
					rotateLock: false,
					color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
						"#ea7ccc"
					],
					padding: [0, 0, 0, 0],
					dataLabel: true,
					enableScroll: false,
					legend: {
						show: true,
						position: "right",
						lineHeight: 25
					},
					title: {
						name: "",
						fontSize: 15,
						color: "#666666"
					},
					subtitle: {
						name: "",
						fontSize: 25,
						color: "#7cb5ec"
					},
					extra: {
						ring: {
							ringWidth: 20,
							activeOpacity: 0.5,
							activeRadius: 10,
							offsetAngle: 0,
							labelWidth: 15,
							border: true,
							borderWidth: 3,
							borderColor: "#FFFFFF"
						}
					}
				},

				// 时间利用分析图表配置
				chartData: {},
				opts: {

					color: ["rgb(249,115,22)", "rgb(229,231,235)", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452",
						"#9A60B4",
						"#ea7ccc"
					],
					padding: [15, 15, 0, 5],
					enableScroll: false,
					dataLabel: false,
					legend: {},
					dataLabel: false,
					xAxis: {
						disableGrid: true,
						axisLineColor: "#FFFFFF",
					},
					yAxis: {
						data: [{
							min: 0
						}],
						disabled: true,
						disableGrid: true,
					},
					extra: {
						column: {
							type: "stack",
							width: 38,
							seriesGap: 2,
							activeBgColor: "#000000",
							activeBgOpacity: 0.08,
							labelPosition: "center",

						}
					}
				},

			};
		},
		onReady() {
			this.getServerData();
			this.getCategorize();
			this.getTimeUse();

		},
		methods: {
			getServerData() {
				setTimeout(() => {
					let res = {
						series: [{
							name: "正确率",
							color: "rgb(251,138,50)",
							data: 0.8
						}]
					};
					this.arcbarData = JSON.parse(JSON.stringify(res));
				}, 500);
			},
			getCategorize() {
				setTimeout(() => {
					let res = {
						series: [{
							data: [{
								"name": "工作",
								"value": 50,
								"labelShow": false
							}, {
								"name": "学习",
								"value": 30,
								"labelShow": false
							}, {
								"name": "锻炼",
								"value": 20,
								"labelShow": false
							}, {
								"name": "跳舞",
								"value": 18
							}, ]
						}]
					};
					this.ringData = JSON.parse(JSON.stringify(res));
				}, 500);
			},

			getTimeUse() {
				setTimeout(() => {
					let res = {
						categories: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"],
						series: [{
								name: "已完成",
								textColor: "#FFFFFF",
								data: [35, 36, 31, 33, 13, 34, 20],
								labelShow: false
							},
							{
								name: "未完成",
								textColor: "#FFFFFF",
								data: [18, 27, 21, 24, 6, 28, 10],
								labelShow: false
							}
						]
					};
					this.chartData = JSON.parse(JSON.stringify(res));
				}, 500);
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

<style scoped>
	@import url("https://at.alicdn.com/t/c/font_4945078_d8s44zrrq5.css");

	.charts-arcbar {
		width: 240rpx;
		height: 220rpx;

	}

	.charts-ring {
		width: 620rpx;
		height: 250rpx;
		position: relative;
		left: -126rpx;

	}

	.flex {
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		padding: 20rpx 0 0 0;
	}

	.container {
		padding: 15px;
		background-color: #fff;

	}

	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 30rpx;
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

	.greeting {
		width: 100%;
		display: flex;
		flex-direction: column;
	}

	.greeting-text {
		font-size: 20px;
		font-weight: bold;
		color: #333;
	}

	.date {
		font-size: 14px;
		color: #999;
		margin: 10rpx 0;
		width: 100%;
	}

	.avatar {
		width: 50px;
		height: 50px;
		border-radius: 25px;
	}

	.date-nav {
		display: flex;
		justify-content: space-around;
		margin: 15px 0;
	}

	.date-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		font-size: 14px;
		color: #999;
	}

	.date-item.active {
		color: #f39726;
	}

	.day {
		font-weight: bold;
	}

	.task-stat-title {
		display: flex;
		flex-direction: column;
		font-size: 16px;
		font-weight: bold;
		color: #333;
		margin: 15px 0;
	}

	.sub-text {
		margin-top: 10rpx;
		font-size: 12px;
		color: #999;
	}

	.task-card {
		background-color: #f8f8f8;
		border-radius: 8px;
		padding: 15px;
		margin-bottom: 15px;
	}

	.chart-title {
		font-size: 14px;
		font-weight: bold;
		color: #333;
	}

	.task-info {
		display: flex;
		flex-direction: column;
		line-height: 40rpx;
		justify-content: center;
		padding-left: 150rpx
	}

	.week-target {
		font-size: 14px;
		color: #333;
		font-weight: bold;
	}

	.completion-rate {
		font-size: 12px;
		color: #62d373;
		margin-top: 3px;
	}

	.completed-task {
		font-size: 12px;
		color: #999;
		margin-top: 3px;
	}

	.category-info {
		margin-top: 10px;
	}

	.category-item {
		display: flex;
		align-items: center;
		margin-bottom: 5px;
	}

	.category-dot {
		width: 10px;
		height: 10px;
		border-radius: 5px;
		margin-right: 5px;
	}

	.time-use-info {
		margin-top: 10px;
		display: flex;
		justify-content: space-between;
		font-size: 12px;
		color: #999;
	}


	.createList {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.create-language {
		margin-top: 32rpx;
		width: 95rpx;
		height: 95rpx;
		background-color: #eee;
		text-align: center;
		line-height: 95rpx;
		border-radius: 50%;
	}

	.create {
		margin-top: 32rpx;
		width: 94rpx;
		height: 94rpx;
		background-color: #eee;
		text-align: center;
		line-height: 95rpx;
		border-radius: 50%;
		font-size: 48rpx;
	}

	.create-task {
		text-align: center;
		margin: 32rpx 10rpx 0 10rpx;
		width: 55%;
		padding: 12px;
		background-color: #f60;
		color: white;
		border: none;
		border-radius: 40rpx;
		font-size: 30rpx;
	}
</style>