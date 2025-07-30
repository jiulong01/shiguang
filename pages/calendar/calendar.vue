<template>
	<view class="content">
		<view class="" style="height: 60rpx;"></view>
		<view class="head">
			<view class="head-left">
				<text class="head-left-hello">{{ greeting }}，Alex</text>
				<text class="head-left-time">{{ fullDateText }}</text>
			</view>
			<view class="greeting-avatar">
				<img src="@/static/task/task-avatar.png" class="greeting-avatar-img" alt="" />
				<view class="greeting-avatar-premium">
					<img src="@/static/vip/vipLogo.png" class="greeting-avatar-premium-img" alt="" />
				</view>
			</view>

		</view>
		<!-- 日历部分 -->
		<view class="calendar-content" :class="calendarTransition" v-if="showCalendar" @touchstart="onTouchStart"
			@touchmove="onTouchMove" @touchend="onTouchEnd">
			<!-- <uni-section title="日历" type="line"></uni-section> -->
			<uni-calendar ref="uniCalendar" class="uni-calendar--hook" :selected="info.selected" :date="info.date"
				:showMonth="false" @change="change" @monthSwitch="monthSwitch" />
		</view>
		<!-- 时间轴部分 -->
		<view class="top-title">
			<text class="title-main">今日安排</text>
			<text class="title-date">{{ fullDateText.split('年')[1] }}</text>
		</view>
		<view class="timeline">
			<view v-for="hour in 15" :key="hour" class="timeline-hour">
				<view class="time-mark">
					{{ `${(hour + 7).toString().padStart(2, '0')}:00` }}
				</view>
				<view class="dashed-line" v-if="hasEventsAt(hour + 7)"></view>
				<view class="events-center">
					<view v-for="event in eventsAtHour(hour + 7)" :key="event.time" class="event-card"
						:style="{borderLeftColor: getCategoryColor(event.type,1),backgroundColor: getCategoryColor(event.type,0.1)}">
						<view class="card-header">
							<view class="card-title">
								<text>{{ event.name }}</text>
								<text v-if="event.star" class="star">★</text>
							</view>
							<view v-if="event.done" class="done-icon">
								<uni-icons type="checkmarkempty" color="#fff" size="16" />
							</view>
						</view>
						<view class="card-info">
							<text class="card-time">{{ event.time }} - {{ event.end }}</text>
							<view class="category">
								<view class="dot" :style="{ backgroundColor: getCategoryColor(event.type,1) }" />
								<text class="category-name">{{ event.type }}</text>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 数据统计卡片 -->
		<view class="data-cards">
			<view class="data-card">
				<qiun-data-charts type="arcbar" :opts="completionOpts" :chartData="completionData"
					background="rgba(0,0,0,0)" />
				<text class="card-label">完成率</text>
			</view>

			<view class="data-card">
				<qiun-data-charts type="column" :opts="trendOpts" :chartData="trendData" background="rgba(0,0,0,0)" />
				<text class="card-label">周趋势</text>
			</view>

			<view class="data-card">
				<qiun-data-charts type="ring" :opts="categoryOpts" :chartData="categoryData"
					background="rgba(0,0,0,0)" />
				<text class="card-label">分类</text>
			</view>
		</view>

		<view style="height: 140rpx;"></view>
		<!-- 新建日程按钮 -->

		<BottomBar></BottomBar>

	</view>
</template>

<script>
	function getDate(date, AddDayCount = 0) {
		if (!date) {
			date = new Date();
		}
		if (typeof date !== "object") {
			date = date.replace(/-/g, "/");
		}
		const dd = new Date(date);

		dd.setDate(dd.getDate() + AddDayCount);

		const y = dd.getFullYear();
		const m =
			dd.getMonth() + 1 < 10 ? "0" + (dd.getMonth() + 1) : dd.getMonth() + 1;
		const d = dd.getDate() < 10 ? "0" + dd.getDate() : dd.getDate();
		return {
			fullDate: y + "-" + m + "-" + d,
			year: y,
			month: m,
			date: d,
			day: dd.getDay(),
		};
	}

	export default {
		onLoad() {
			for (let key in this.colorList) {
				this.info.selected.push({
					date: key,
					info: "",
					colorList: this.colorList[key]
				})
			}
		},
		data() {
			return {
				showCalendar: false,
				info: {
					lunar: true,
					range: true,
					insert: false,
					selected: [],
				},
				touchStartX: 0,
				touchEndX: 0,
				calendarTransition: '', // 添加动画状态变量
				// 时间轴数据
				currentDate: getDate(new Date()).fullDate, // 当前选中日期
				date: [1], // 当天任务
				eventMap: {
					'2025-07-30': [{
							time: "08:00",
							end: "09:00",
							name: "早餐会议",
							type: "工作",
							star: false,
							done: true
						},
						{
							time: "08:30",
							end: "09:00",
							name: "会议",
							type: "学习",
							star: false,
							done: true
						},
						{
							time: "14:00",
							end: "15:30",
							name: "完成产品设计方案",
							type: "工作",
							star: true,
							done: false
						},
						{
							time: "18:30",
							end: "19:30",
							name: "健身房锻炼",
							type: "健康",
							star: false,
							done: false
						},
						{
							time: "20:00",
							end: "21:00",
							name: "阅读《原子习惯》",
							type: "学习",
							star: true,
							done: true
						}
					],
					'2025-06-11': [{
						time: "10:00",
						end: "11:00",
						name: "项目讨论",
						type: "工作",
						star: false,
						done: false
					}]
				},
				colorList: {
					'2025-06-16': [{
							color: 'red',
							lei: '工作'
						},
						{
							color: 'blue',
							lei: '读书'
						},
						{
							color: 'green',
							lei: '运动'
						}
					],
					'2025-06-17': [{
							color: 'red',
							lei: '工作'
						},
						{
							color: 'blue',
							lei: '读书'
						}
					]
				},
				// 完成率图表数据
				completionData: {
					series: [{
						name: "完成率",
						color: "#FF6B00",
						data: 0
					}]
				},
				completionOpts: {
					padding: [0, 0, 0, 0],
					title: {
						name: "10%",
						fontSize: 15,
						color: "#333333"
					},
					subtitle: {
						name: "",
					},
					extra: {
						arcbar: {
							type: "circle",
							width: 8,
							backgroundColor: "#E9E9E9",
							startAngle: 1.5,
							endAngle: 0.25,
							gap: 2
						}
					}
				},

				// 修改周趋势图表数据
				trendData: {
					categories: ["一", "二", "三", "四", "五", "六", "日"],
					series: [{
							name: "已完成",
							data: null,
							color: "#FF6B00"
						},
						{
							name: "未完成",
							data: null,
							color: "#E0E0E0"
						}
					]
				},
				trendOpts: {
					color: ["#FF6B00", "#E0E0E0"], // 与 series 中颜色保持一致也行
					padding: [15, 15, 0, 5],
					enableScroll: false,
					dataLabel: false, // 禁用数据标签
					legend: {
						show: false
					}, // 不显示图例
					xAxis: {
						disabled: true, // 不显示 X 轴
						disableGrid: true,
						axisLineColor: "#FFFFFF"
					},
					yAxis: {
						disabled: true, // 不显示 Y 轴
						disableGrid: true,
						data: [{
							min: 0
						}]
					},
					extra: {
						column: {
							type: "stack",
							width: 38,
							// seriesGap: 4,
							categoryGap: 2,
							// 删除 activeBgColor 和 activeBgOpacity，避免图表变透明
							labelPosition: "center"
						}
					},
					tooltip: {
						show: false // 禁用 tooltip 提示
					}
				},

				// 修改分类图表数据
				categoryData: {
					series: [{
						name: "工作",
						data: 30
					}, {
						name: "学习",
						data: 30
					}, {
						name: "健康",
						data: 30
					}, {
						name: "其他",
						data: 10
					}]
				},
				categoryOpts: {
					rotate: false,
					rotateLock: false,
					color: ["#f56c6c", "#409eff", "#67c23a", "#909399", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
						"#ea7ccc"
					],
					padding: [0, 0, 0, 0],
					dataLabel: false,
					enableScroll: false,
					legend: {
						show: false
					},
					title: {
						name: ""
					},
					subtitle: {
						name: ""
					},
					extra: {
						ring: {
							ringWidth: 8,
							offsetAngle: 0,
							border: false,
						}
					}
				},
			};
		},
		onReady() {
			this.$nextTick(() => {
				this.showCalendar = true;
				// 初始化统计数据
				setTimeout(() => {
					this.updateStatistics();
				}, 500);
			});
		},
		computed: {
			events() {
				return this.eventMap[this.currentDate] || [];
			},
			// 问候语（早上好 / 下午好 / 晚上好）
			greeting() {
				const hours = new Date().getHours();
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
			// 当前日期完整显示：2025年6月10日 星期二
			fullDateText() {
				const d = new Date(this.currentDate);
				const days = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
				return `${d.getFullYear()}年${d.getMonth() + 1}月${d.getDate()}日 ${days[d.getDay()]}`;
			},
		},
		methods: {
			// 检查该时间段是否有事件 如果有事件则返回true 该时间段上显示虚线垂直线
			hasEventsAt(hour) {
				return this.events.some(e => parseInt(e.time.split(':')[0]) === hour);
			},
			// 检查该时间段的所有事件 并返回
			eventsAtHour(hour) {
				return this.events.filter(e => parseInt(e.time.split(':')[0]) === hour);
			},
			// 转换函数：十六进制 → RGBA
			hexToRgba(hex, opacity = 1) {
				const r = parseInt(hex.slice(1, 3), 16);
				const g = parseInt(hex.slice(3, 5), 16);
				const b = parseInt(hex.slice(5, 7), 16);
				return `rgba(${r}, ${g}, ${b}, ${opacity})`;
			},
			getCategoryColor(type, opcity) {
				switch (type) {
					case '工作':
						return this.hexToRgba("#f56c6c", opcity);
					case '学习':
						return this.hexToRgba("#409eff", opcity);
					case '健康':
						return this.hexToRgba("#67c23a", opcity);;
					default:
						return this.hexToRgba("#909399", opcity);;
				}
			},
			change(e) {
				const selectedDate = e.fulldate;
				this.currentDate = selectedDate;
				this.info.date = selectedDate;
				this.info.selected.push({
					date: selectedDate,
					info: "",
				});

				// 更新统计数据
				this.$nextTick(() => {
					this.updateStatistics();
				});
			},
			// 当月份切换时触发
			monthSwitch(e) {
				const m = e.month < 10 ? "0" + e.month : e.month;
				const newMonth = `${e.year}-${m}`;
				// 只有在月份变化时才切换到1号
				if (!this.info.date || !String(this.info.date).startsWith(newMonth)) {
					this.info.date = `${newMonth}-01`;
				}
			},
			// 触摸开始事件处理
			onTouchStart(e) {
				// 记录触摸开始的X坐标
				this.touchStartX = e.touches[0].clientX;
			},
			// 触摸移动事件处理
			onTouchMove(e) {
				// 记录触摸移动时的X坐标
				this.touchEndX = e.touches[0].clientX;
			},
			// 触摸结束事件处理
			onTouchEnd() {
				if (this.touchEndX === 0) return
				const deltaX = this.touchEndX - this.touchStartX;
				// 判断滑动距离是否超过阈值(50像素)
				if (Math.abs(deltaX) > 50) {
					// 滑动距离阈值
					this.calendarTransition = deltaX < 0 ? 'slide-left' : 'slide-right';
					if (deltaX < 0) {
						this.nextMonth();
					} else {
						this.prevMonth();
					}
				}
				setTimeout(() => {
					this.calendarTransition = '';
				}, 300);
				this.touchStartX = 0;
				this.touchEndX = 0;
			},
			// 动画结束后重置动画状态
			onTransitionEnd() {
				this.calendarTransition = '';
			},
			// 切换到上个月的方法
			prevMonth() {
				this.$refs.uniCalendar && this.$refs.uniCalendar.pre()
			},
			// 切换到下个月的方法
			nextMonth() {
				this.$refs.uniCalendar && this.$refs.uniCalendar.next()
			},

			// 计算完成率并更新图表
			calculateCompletionRate() {
				console.log('111');
				const eventList = this.eventMap[this.currentDate] || [];
				if (eventList.length === 0) {
					this.completionData.series[0].data = 0;
					this.completionOpts.title.name = '0%';
					return
				};
				const doneCount = eventList.filter(event => event.done).length;
				const rate = Math.round((doneCount / eventList.length) * 100);
				// 更新图表数据（如果需要）
				this.completionData.series[0].data = rate / 100;
				this.completionOpts.title.name = `${rate}%`;
				return rate;
			},

			// 计算周趋势数据
			calculateWeekTrend() {
				// 1. 获取当前周的日期范围（周一到周日）
				const currentDate = new Date(this.currentDate);
				const day = currentDate.getDay();
				const monday = new Date(currentDate);
				monday.setDate(currentDate.getDate() - (day === 0 ? 6 : day - 1)); // 调整到本周一
				// 2. 初始化本周每天的数据
				const planTasks = [];
				const completedTasks = [];
				// 3. 遍历本周7天
				for (let i = 0; i < 7; i++) {
					const date = new Date(monday);
					date.setDate(monday.getDate() + i);
					const dateKey = date.getFullYear() + '-' +
						String(date.getMonth() + 1).padStart(2, '0') + '-' +
						String(date.getDate()).padStart(2, '0');

					// 4. 从eventMap获取当天数据
					const dailyEvents = this.eventMap[dateKey] || [];
					planTasks.push(dailyEvents.length); // 当天总任务数
					completedTasks.push(dailyEvents.filter(event => event.done).length); // 当天已完成数
				}
				// 5. 更新图表数据（使用新对象触发响应式更新）
				this.trendData.series[0].data = completedTasks
				this.trendData.series[1].data = planTasks.map((total, i) => total - completedTasks[i])

			},

			// 修改计算分类数据的方法
			calculateCategoryData() {
				// 统计不同类型的任务数量
				const typeCount = {};
				if (this.events && this.events.length > 0) {
					this.events.forEach(event => {
						if (!typeCount[event.type]) {
							typeCount[event.type] = 0;
						}
						typeCount[event.type]++;
					});
				}

				// 转换为图表数据格式
				const series = Object.keys(typeCount).map(type => {
					return {
						name: type,
						data: typeCount[type]
					};
				});

				// 如果没有数据，使用默认数据
				if (series.length === 0) {
					this.categoryData.series = [{
						name: "工作",
						data: 30
					}, {
						name: "学习",
						data: 30
					}, {
						name: "健康",
						data: 30
					}, {
						name: "其他",
						data: 10
					}];
				} else {
					this.categoryData.series = series;
				}
			},

			// 更新所有统计数据
			updateStatistics() {
				this.calculateCompletionRate();
				this.calculateWeekTrend();
				this.calculateCategoryData();
			}
		},
	};
</script>

<style lang="less" scoped>
	@import url("https://at.alicdn.com/t/c/font_4945078_d8s44zrrq5.css");

	.content {
		margin: 0 30rpx;
		padding-top: 40rpx;
	}

	.head {
		margin-bottom: 30rpx;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		align-items: center;

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

	.head-left {
		display: flex;
		flex-direction: column;
		justify-content: center;
		height: 100%;
	}

	.head-left-hello {
		font-size: 20px;
		font-weight: bold;
		color: #333;
	}

	.head-left-time {
		font-size: 25rpx;
		color: #666;
	}

	/* 日历样式 */
	.calendar-content {
		margin-top: 30rpx;
		position: relative;
	}

	.uni-calendar--hook {
		width: 100%;
	}

	.uni-calendar--hook ::v-deep .uni-calendar__weeks-item {
		background-color: #f1f5f9;
		margin: 1px;
		width: 47px;
		height: 47px;
		border-radius: 9px;
		overflow: hidden;
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		align-items: center;
		position: relative;
	}

	.uni-calendar--hook ::v-deep .uni-calendar-item__weeks-box-circle {
		top: 64rpx;
		right: 42rpx;
	}

	/* 为了提高性能，启用硬件加速 */
	.uni-calendar--hook ::v-deep .uni-calendar__weeks-item {
		will-change: transform, opacity;
	}

	/* 日历容器的滑动动画 */
	.slide-left .uni-calendar--hook ::v-deep .uni-calendar__weeks-item {
		animation: slideLeft 0.2s ease-out forwards;
		/* 缩短动画时间并使用ease-out */
	}

	.slide-right .uni-calendar--hook ::v-deep .uni-calendar__weeks-item {
		animation: slideRight 0.2s ease-out forwards;
		/* 缩短动画时间并使用ease-out */
	}

	/* 向左滑动时的动画 */
	@keyframes slideLeft {
		from {
			transform: translateX(20px);
			opacity: 0.5;
		}

		to {
			transform: translateX(0);
			opacity: 1;
		}
	}

	/* 向右滑动时的动画 */
	@keyframes slideRight {
		from {
			transform: translateX(-20px);
			opacity: 0.5;
		}

		to {
			transform: translateX(0);
			opacity: 1;
		}
	}

	/* 时间轴样式 */
	.timeline {
		margin-top: 30rpx;
		background-color: #F1F5F9;
		border-radius: 24rpx;
		padding-top: 20rpx;
	}

	.timeline-hour {
		display: flex;
		position: relative;
		min-height: 100rpx;
		padding-left: 120rpx; // 留出足够空间放时间标记 + 虚线
		padding-bottom: 20rpx;
	}

	.time-mark {
		position: absolute;
		left: 20rpx;
		top: 0;
		width: 100rpx; // 调整为与 padding-left 一致
		font-size: 24rpx;
		color: #666;
		text-align: center; // 居中时间文本
	}

	.dashed-line {
		position: absolute;
		left: 70rpx; // 保持水平居中
		top: 36rpx; // 增加顶部偏移（根据你的时间字体大小可调）
		width: 1px;
		height: calc(100% - 36rpx); // 减去上移部分，保持高度一致
		border-left: 1px dashed #ccc;
	}


	.events {
		flex: 1;
		padding-left: 20rpx;
	}

	.event-card {
		background-color: #fff;
		border-left: 8rpx solid #ccc;
		padding: 30rpx;
		border-radius: 20rpx;
		margin-bottom: 20rpx;
		box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.08);
		height: 100rpx;
	}

	.events-center {
		margin-left: 30rpx;
		width: 90%; // 宽度扩大
	}

	.card-title {
		display: flex;
		justify-content: space-between;
		font-weight: 600;
		font-size: 28rpx;
		color: #333;
	}

	.star {
		color: #f90;
		margin-left: 10rpx;
	}

	.card-info {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-top: 10rpx;
		font-size: 24rpx;
		color: #666;
	}

	.category {
		display: flex;
		align-items: center;
	}

	.dot {
		width: 16rpx;
		height: 16rpx;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.category-name {
		font-size: 22rpx;
		color: #666;
	}

	.top-title {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin: 20rpx 0;
	}

	.title-main {
		font-size: 40rpx;
		font-weight: bold;
		color: #333;
	}

	.title-date {
		font-size: 26rpx;
		color: #999;
	}

	.card-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.done-icon {
		width: 32rpx;
		height: 32rpx;
		border-radius: 50%;
		background-color: #555; // 浅黑色
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.data-cards {
		display: flex;
		justify-content: space-between;
		margin-top: 30rpx;
	}

	.data-card {
		width: 30%;
		height: 240rpx;
		background-color: #f1f5f9;
		border-radius: 20rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 20rpx;
		/* 减少内边距 */
		overflow: visible;
		/* 允许内容溢出 */
		box-sizing: border-box;
		pointer-events: none;
	}

	.card-label {
		margin-top: 10rpx;
		font-size: 28rpx;
		color: #666;
		text-align: center;
	}
</style>