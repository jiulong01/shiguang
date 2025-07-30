<template>
	<view class="page-container">
		<view class="" style="height: 70rpx;"></view>
		<view class="header">
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

		<!-- 日期选择 -->
		<DateSelector></DateSelector>

		<!-- 四象限任务管理 -->
		<view class="task-stat-title">
			<text>四象限任务管理</text>
			<text class="sub-text">按重要性和紧急性分类</text>
		</view>

		<view class="quadrants">
			<!-- 紧急且重要象限 -->
			<transition name="quadrant-expand">
				<view class="quadrant urgent-important" :class="{
            expanded: expandedQuadrant === 'urgentImportant',
            hidden: expandedQuadrant && expandedQuadrant !== 'urgentImportant',
          }">
					<h4>紧急且重要</h4>
					<draggable class="draggable" v-model="tasks.urgentImportant" group="tasks" :animation="400"
						ghost-class="ghost-card" drag-class="drag-card" chosen-class="chosen-card" :delay="50"
						:delayOnTouchOnly="true" :easing="'cubic-bezier(0.2, 0, 0.2, 1)'" @start="onDragStart"
						@end="onDragEnd" @add="onDragAdd" :sort="true" :move="checkMove" :disabled="false"
						:clone="cloneTask">
						<view class="task-card" v-for="(task, index) in tasks.urgentImportant" :key="task.index"
							:data-id="task.id" @click.stop="handleTaskClick(task)">
							<view class="task-card-title">
								<view :class="{
                    'task-card-title-content action': task.isFinished,
                    'task-card-title-content': !task.isFinished,
                  }">
									{{ task.title }}
								</view>
								<view :class="{
                    'task-card-title-yes finished': task.isFinished,
                    'task-card-title-yes': !task.isFinished,
                  }">
									✔︎
								</view>
							</view>
							<view class="time">🕒 {{ task.time }}</view>
						</view>
						<view v-if="tasks.urgentImportant.length === 0" class="task-card empty-placeholder">
							<view class="task-card-title">
								<view class="task-card-title-content">拖放任务到这里</view>
							</view>
						</view>
					</draggable>
				</view>
			</transition>

			<!-- 重要不紧急象限 -->
			<transition name="quadrant-expand">
				<view class="quadrant important-not-urgent"
					:class="{
            expanded: expandedQuadrant === 'importantNotUrgent',hidden:expandedQuadrant && expandedQuadrant !== 'importantNotUrgent',}">
					<h4>重要不紧急</h4>
					<draggable class="draggable" v-model="tasks.importantNotUrgent" group="tasks" :animation="400"
						ghost-class="ghost-card" drag-class="drag-card" chosen-class="chosen-card" :delay="50"
						:delayOnTouchOnly="true" :easing="'cubic-bezier(0.2, 0, 0.2, 1)'" @start="onDragStart"
						@end="onDragEnd" @add="onDragAdd" :sort="true" :move="checkMove" :disabled="false"
						:clone="cloneTask">
						<view class="task-card" v-for="(task, index) in tasks.importantNotUrgent" :key="index"
							:data-id="task.id" @click.stop="handleTaskClick(task)">
							<view class="task-card-title">
								<view :class="{
                    'task-card-title-content action': task.isFinished,'task-card-title-content': !task.isFinished,}">
									{{ task.title }} {{ task.id }}
								</view>
								<view :class="{
                    'task-card-title-yes finished': task.isFinished,'task-card-title-yes': !task.isFinished,}">
									✔︎
								</view>
							</view>
							<view class="time">🕒 {{ task.time }}</view>
						</view>
						<view v-if="tasks.importantNotUrgent.length === 0" class="task-card empty-placeholder">
							<view class="task-card-title">
								<view class="task-card-title-content">拖放任务到这里</view>
							</view>
						</view>
					</draggable>
				</view>
			</transition>

			<!-- 紧急不重要象限 -->
			<transition name="quadrant-expand">
				<view class="quadrant urgent-not-important" :class="{
            expanded: expandedQuadrant === 'urgentNotImportant',
            hidden:
              expandedQuadrant && expandedQuadrant !== 'urgentNotImportant',
          }">
					<h4>紧急不重要</h4>
					<draggable class="draggable" v-model="tasks.urgentNotImportant" group="tasks" :animation="400"
						ghost-class="ghost-card" drag-class="drag-card" chosen-class="chosen-card" :delay="50"
						:delayOnTouchOnly="true" :easing="'cubic-bezier(0.2, 0, 0.2, 1)'" @start="onDragStart"
						@end="onDragEnd" @add="onDragAdd" :sort="true" :move="checkMove" :disabled="false"
						:clone="cloneTask">
						<view class="task-card" v-for="(task, index) in tasks.urgentNotImportant" :key="index"
							:data-id="task.id" @click.stop="handleTaskClick(task)">
							<view class="task-card-title">
								<view :class="{
                    'task-card-title-content action': task.isFinished,'task-card-title-content': !task.isFinished,}">
									{{ task.title }}
								</view>
								<view :class="{
                    'task-card-title-yes finished': task.isFinished,'task-card-title-yes': !task.isFinished,}">
									✔︎
								</view>
							</view>
							<view class="time">🕒 {{ task.time }}</view>
						</view>
						<view v-if="tasks.urgentNotImportant.length === 0" class="task-card empty-placeholder">
							<view class="task-card-title">
								<view class="task-card-title-content">拖放任务到这里</view>
							</view>
						</view>
					</draggable>
				</view>
			</transition>

			<!-- 不紧急不重要象限 -->
			<transition name="quadrant-expand">
				<view class="quadrant not-urgent-not-important" :class="{
            expanded: expandedQuadrant === 'notUrgentNotImportant',
            hidden:
              expandedQuadrant && expandedQuadrant !== 'notUrgentNotImportant',
          }">
					<h4>不紧急不重要</h4>
					<draggable class="draggable" v-model="tasks.notUrgentNotImportant" group="tasks" :animation="400"
						ghost-class="ghost-card" drag-class="drag-card" chosen-class="chosen-card" :delay="50"
						:delayOnTouchOnly="true" :easing="'cubic-bezier(0.2, 0, 0.2, 1)'" @start="onDragStart"
						@end="onDragEnd" @add="onDragAdd" :sort="true" :move="checkMove" :disabled="false"
						:clone="cloneTask">
						<view class="task-card" v-for="(task, index) in tasks.notUrgentNotImportant" :key="index"
							:data-id="task.id" @click.stop="handleTaskClick(task)">
							<view class="task-card-title">
								<view :class="{
                    'task-card-title-content action': task.isFinished,
                    'task-card-title-content': !task.isFinished,
                  }">
									{{ task.title }}
								</view>
								<view :class="{
                    'task-card-title-yes finished': task.isFinished,
                    'task-card-title-yes': !task.isFinished,
                  }">✔︎
								</view>
							</view>
							<view class="time">🕒 {{ task.time }}</view>
						</view>
						<view v-if="tasks.notUrgentNotImportant.length === 0" class="task-card empty-placeholder">
							<view class="task-card-title">
								<view class="task-card-title-content">拖放任务到这里</view>
							</view>
						</view>
					</draggable>
				</view>
			</transition>
		</view>

		<!-- 任务分布统计 -->
		<view class="stats">
			<view>任务分布</view>
			<view class="stats-content">
				<view class="">
					<i class="iconfont icon-luyin" style="color: #FF8A65;"></i>
					紧急且重要: {{ getUnfinishedTaskCount(tasks.urgentImportant) }}
				</view>
				<view class="">
					<i class="iconfont icon-luyin" style="color: #4CAF50;"></i>
					重要不紧急: {{ getUnfinishedTaskCount(tasks.importantNotUrgent) }}
				</view>
				<view class="">
					<i class="iconfont icon-luyin" style="color: #42A5F5;"></i>
					紧急不重要: {{ getUnfinishedTaskCount(tasks.urgentNotImportant) }}
				</view>
				<view class="">
					<i class="iconfont icon-luyin" style="color: #AB47BC;"></i>
					不紧急不重要: {{ getUnfinishedTaskCount(tasks.notUrgentNotImportant) }}
				</view>
			</view>
		</view>

		<!-- 高级会员 -->

		<Member></Member>

		<!-- 补位盒子 -->
		<view class="lastTab"></view>

		<!-- 自定义确认弹框 -->
		<view class="custom-modal" v-if="isModalVisible">
			<view class="modal-content">
				<p>{{ modalMessage }}</p>
				<view class="modal-buttons">
					<button @click="confirmRestoreTask()">确定</button>
					<button @click="cancelRestoreTask()">取消</button>
				</view>
			</view>
		</view>
		<!-- 底部新建任务按钮 -->
		<BottomBar></BottomBar>
	</view>
</template>

<script>
	// import DateSelector from "@/components/DateSelector/DateSelector.vue";
	import Member from '../../components/Member/Member.vue';
	import BottomBar from "../../components/BottomBar/BottomBar.vue"
	import calendar from '../../components/calendar/calendar.vue';
	export default {

		data() {
			return {
				// 拖拽相关数据
				draggedItemId: null, // 记录当前拖拽的项目ID
				dragSourceQuadrant: null, // 记录拖拽源象限
				currentTime: new Date(),
				// 基础数据
				selectedDay: 3,
				dateOffset: 0,

				// 四象限任务数据
				tasks: {
					urgentImportant: [{
							id: 1,
							title: "完成产品设计方案",
							time: "今天 14:00",
							isFinished: false,
						},
						{
							id: 2,
							title: "阅读《原子习惯》第三章",
							time: "今天 20:00",
							isFinished: false,
						},
						{
							id: 10,
							title: "阅读《原子物理》第三章",
							time: "今天 15:00",
							isFinished: false,
						},
					],
					importantNotUrgent: [{
						id: 3,
						title: "健身房锻炼",
						time: "今天 18:30",
						isFinished: false,
					}, ],
					urgentNotImportant: [{
						id: 4,
						title: "购买生日礼物",
						time: "明天 12:00",
						isFinished: false,
					}, ],
					notUrgentNotImportant: [{
							id: 5,
							title: "回复工作邮件",
							time: "昨天 17:00",
							isFinished: false,
						},
						{
							id: 6,
							title: "回复消息",
							time: "昨天 17:00",
							isFinished: false,
						},
					],
				},

				// 弹框相关
				isModalVisible: false, // 弹框的显示与隐藏
				modalMessage: "", // 弹框的标题
				currentTask: null, // 当前操作的任务

				// 象限展开状态
				expandedQuadrant: null, // 记录被放大的象限标识
			};
		},
		methods: {
			/**
			 * 检查移动是否允许
			 * @param {Object} evt - 拖拽事件对象
			 * @return {Boolean} - 返回true表示允许移动
			 */
			checkMove(evt) {
				console.log("检查移动", evt);
				return true; // 允许所有移动
			},

			/**
			 * 克隆任务（用于跨象限拖拽）
			 * @param {Object} task - 任务对象
			 * @return {Object} - 返回克隆后的任务对象
			 */
			cloneTask(task) {
				console.log("克隆任务", task);
				console.log({
					...task
				});
				// 返回一个新的任务对象，避免引用问题
				return {
					...task
				}; // 返回一个新的任务对象，避免引用问题
			},

			/**
			 * 处理任务点击事件
			 * @param {Object} task - 被点击的任务
			 */
			handleTaskClick(task) {
				if (task.isFinished) {
					// 如果任务已完成，弹出恢复确认框
					this.currentTask = task;
					this.modalMessage = `是否恢复任务：${task.title}`;
					this.isModalVisible = true;
				} else {
					// 如果任务未完成，标记为已完成
					task.isFinished = true;
				}
			},

			/**
			 * 确认恢复任务
			 */
			confirmRestoreTask() {
				if (this.currentTask) {
					this.currentTask.isFinished = false;
				}
				this.isModalVisible = false;
			},

			/**
			 * 取消恢复任务
			 */
			cancelRestoreTask() {
				this.isModalVisible = false;
			},

			/**
			 * 获取未完成任务数量
			 * @param {Array} taskList - 任务列表
			 * @return {Number} - 未完成任务数量
			 */
			getUnfinishedTaskCount(taskList) {
				return taskList.filter((task) => !task.isFinished).length;
			},

			/**
			 * 跳转到VIP页面
			 */
			toVip() {
				uni.navigateTo({
					url: "/pages/test/test",
				});
			},

			/**
			 * 新建任务
			 */
			toCreateTask() {
				uni.navigateTo({
					url: '/pages/task/createTask/createTask'
				});
			},

			/**
			 * 拖拽开始事件处理
			 * @param {Object} evt - 拖拽事件对象
			 */
			onDragStart(evt) {
				console.log("开始拖拽", evt);
				// 保存被拖拽元素的ID
				if (evt.item && evt.item.getAttribute) {
					const taskId =
						evt.item.getAttribute("data-id") ||
						(evt.item.dataset ? evt.item.dataset.id : null);
					this.draggedItemId = taskId;
				}

				// 保存源象限信息
				this.dragSourceQuadrant = this.getQuadrantFromElement(evt.from);

				// 添加拖拽开始的视觉效果
				document.body.classList.add("dragging-active");

				// 播放轻微的触觉反馈（如果平台支持）
				if (window.navigator && window.navigator.vibrate) {
					window.navigator.vibrate(50);
				}
			},

			/**
			 * 拖拽结束事件处理
			 * @param {Object} evt - 拖拽事件对象
			 */
			onDragEnd(evt) {
				console.log("拖拽结束", evt);

				try {
					const targetQuadrant = this.getQuadrantFromElement(evt.to);
					console.log(`当前任务象限:${this.dragSourceQuadrant}`);
					console.log(targetQuadrant);
					console.log(`移动到的任务象限:${targetQuadrant}`);
					// 如果跨象限拖拽，可以添加特殊处理
					if (
						this.dragSourceQuadrant &&
						targetQuadrant &&
						this.dragSourceQuadrant !== targetQuadrant
					) {
						console.log(
							`任务从 ${this.dragSourceQuadrant} 移动到 ${targetQuadrant}`
						);
						// 这里可以添加任务移动的业务逻辑

						// 播放成功的触觉反馈
						if (window.navigator && window.navigator.vibrate) {
							window.navigator.vibrate([40, 30, 40]);
						}
					}
				} catch (error) {
					console.error("拖拽结束处理错误:", error);
				}

				// 移除拖拽状态
				document.body.classList.remove("dragging-active");

				// 重置拖拽相关变量
				this.draggedItemId = null;
				this.dragSourceQuadrant = null;
			},

			/**
			 * 新元素添加事件处理
			 * @param {Object} evt - 拖拽事件对象
			 */
			onDragAdd(evt) {
				console.log("添加新元素", evt);

				try {
					// 添加元素到新象限的动画效果
					const newElement = evt.item;
					if (newElement) {
						// 添加一个临时类，然后移除它以触发CSS动画
						newElement.classList.add("task-card-added");
						setTimeout(() => {
							newElement.classList.remove("task-card-added");
						}, 500);
					}
				} catch (error) {
					console.error("添加新元素处理错误:", error);
				}
			},

			/**
			 * 从DOM元素确定所属象限
			 * @param {HTMLElement} element - DOM元素
			 * @return {String|null} - 象限标识或null
			 */
			getQuadrantFromElement(element) {
				try {
					if (!element) return null;

					// 通过父元素的类名判断象限
					const quadrantElement = element.closest(".quadrant");
					if (!quadrantElement) return null;

					if (quadrantElement.classList.contains("urgent-important")) {
						return "urgentImportant";
					} else if (quadrantElement.classList.contains("important-not-urgent")) {
						return "importantNotUrgent";
					} else if (quadrantElement.classList.contains("urgent-not-important")) {
						return "urgentNotImportant";
					} else if (
						quadrantElement.classList.contains("not-urgent-not-important")
					) {
						return "notUrgentNotImportant";
					}
				} catch (error) {
					console.error("获取象限信息错误:", error);
				}

				return null;
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
	@import url("https://at.alicdn.com/t/c/font_4986921_kdgybha0sf8.css");
	@import url("https://at.alicdn.com/t/c/font_4945078_d8s44zrrq5.css");

	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		list-style: none;
	}

	li {
		margin-left: 40rpx;
	}

	h4 {
		height: 100rpx;
		line-height: 100rpx;
	}

	.page-container {
		padding: 32rpx;
		font-family: Arial, sans-serif;
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

	/* 标头用户样式 */
	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 30rpx;
	}

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

	.section-title {
		font-weight: bold;
		margin-top: 20rpx;
	}

	/* 四象限布局样式 */
	.quadrants {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 24rpx;
		margin-top: 24rpx;

	}

	.quadrant {
		background: #f9f9f9;
		padding: 24rpx;
		border-radius: 20rpx;
		transition: all 0.3s ease;
		position: relative;
		overflow: hidden;
		background-color: #F1F5F9;
	}

	/* 添加这个样式 */
	.quadrant>.draggable {
		display: block;
		min-height: 250rpx;
		cursor: move;
		/* 使整个区域显示为可拖动 */
	}

	.quadrant.expanded {
		grid-column: 1 / -1;
		grid-row: 1 / -1;
	}

	.quadrant.hidden {
		display: none;
	}

	/* 象限颜色标识 */
	.urgent-important {
		border-left: 8rpx solid #ff5722;
	}

	.important-not-urgent {
		border-left: 8rpx solid #4caf50;
	}

	.urgent-not-important {
		border-left: 8rpx solid #2196f3;
	}

	.not-urgent-not-important {
		border-left: 8rpx solid #9c27b0;
	}

	/* 任务卡片样式 */
	.task-card {
		background-color: white;
		margin-top: 12rpx;
		padding: 16rpx;
		border-radius: 16rpx;
		box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.05);
		cursor: move;
		/* 显示可拖动的光标 */
		transition: all 0.3s cubic-bezier(0.2, 0, 0.2, 1);
		position: relative;
		z-index: 1;
		user-select: none;
		touch-action: none;
	}

	.task-card:hover {
		transform: translateY(-2px);
		box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.1);
	}

	.action {
		text-decoration: line-through;
		color: #909090;
	}

	.task-card-title {
		display: flex;
		justify-content: space-between;
		font-size: 26rpx;
	}

	.task-card-title-content {
		width: 200rpx;
	}

	.task-card-title-yes {
		width: 40rpx;
		height: 40rpx;
		text-align: center;
		line-height: 40rpx;
		border-radius: 50%;
		border: 1rpx solid black;
	}

	.finished {
		background-color: #ff5722;
		color: #f9f9f9;
		border-color: orange;
	}

	.task-card .time {
		font-size: 24rpx;
		color: #888;
		margin-top: 8rpx;
	}

	/* 统计区域样式 */
	.stats {
		margin-top: 40rpx;
		margin-bottom: 30rpx;
		font-size: 28rpx;
		background-color: #F1F5F9;
		border-radius: 18rpx;
		padding: 25rpx;
		color: #666666;

		&-content {
			display: flex;
			max-width: 600rpx;
			margin-bottom: 6rpx;
			justify-content: space-between;
			flex-wrap: wrap;
			-webkit-line-clamp: 2;
		}
	}


	/* 创建任务区域样式 */
	.createList {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}


	/* 弹框样式 */
	.custom-modal {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.5);
		backdrop-filter: blur(10rpx);
		display: flex;
		justify-content: center;
		align-items: center;
		z-index: 9999;
	}

	.modal-content {
		background-color: white;
		padding: 80rpx;
		border-radius: 48rpx;
		text-align: center;
		box-shadow: 0 20rpx 60rpx rgba(0, 0, 0, 0.2);
		width: 80%;
		max-width: 800rpx;
	}

	.modal-content p {
		font-size: 28rpx;
		color: #333;
		margin-bottom: 60rpx;
	}

	.modal-buttons {
		display: flex;
		justify-content: center;
		margin-top: 40rpx;
	}

	.modal-buttons button {
		margin: 0 30rpx;
		padding: 32rpx 64rpx;
		border: none;
		border-radius: 48rpx;
		cursor: pointer;
		font-size: 28rpx;
		transition: all 0.3s ease;
	}

	.modal-buttons button:first-child {
		background-color: #f60;
		color: white;
	}

	.modal-buttons button:first-child:hover {
		background-color: #ff7700;
	}

	.modal-buttons button:last-child {
		background-color: #eee;
		color: #333;
	}

	.modal-buttons button:last-child:hover {
		background-color: #ddd;
	}

	/* 拖拽相关样式 */
	.ghost-card {
		opacity: 0.5;
		background: #e8f4fd;
		border: 2px dashed #4a9eff;
		animation: pulse 1.5s infinite ease-in-out;
		box-shadow: none;
		transform: scale(0.98);
	}

	.drag-card {
		transform: rotate(3deg) scale(1.05);
		box-shadow: 0 10rpx 20rpx rgba(0, 0, 0, 0.15);
		opacity: 0.9;
		z-index: 1000;
	}

	.chosen-card {
		background-color: #f8f8f8;
		box-shadow: 0 0 0 2px #4a9eff;
	}

	.task-card-added {
		animation: highlight-new 0.5s ease-out;
	}

	/* 拖拽过程中的全局样式 */
	:global(body.dragging-active) {
		cursor: grabbing;
	}

	/* 脉动动画，用于占位元素 */
	@keyframes pulse {
		0% {
			opacity: 0.5;
		}

		50% {
			opacity: 0.7;
		}

		100% {
			opacity: 0.5;
		}
	}

	/* 新元素高亮动画 */
	@keyframes highlight-new {
		0% {
			background-color: #fffde7;
			transform: scale(1.05);
		}

		100% {
			background-color: white;
			transform: scale(1);
		}
	}

	/* 拖拽时的吸附线指示器 */
	.snap-indicator {
		position: absolute;
		background-color: rgba(74, 158, 255, 0.5);
		pointer-events: none;
		z-index: 100;
		transition: opacity 0.2s ease;
	}

	.snap-indicator-horizontal {
		height: 2px;
		left: 0;
		right: 0;
	}

	.snap-indicator-vertical {
		width: 2px;
		top: 0;
		bottom: 0;
	}

	.empty-placeholder {
		border: 2px dashed #ccc;
		background-color: #f9f9f9;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #999;
		min-height: 80rpx;
	}

	.lastTab {
		height: 100rpx;
	}
</style>