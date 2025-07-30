<template>
	<view class="container">
		<view class="margin">

		</view>
		<view class="header-top"> </view>
		<!-- 顶部导航栏 -->
		<view class="header">
			<view class="back" @click="goBack">
				<i class="iconfont icon-fanhui"></i>
			</view>
			<view class="header-title">新建任务</view>
			<view class="header-actions">
				<text class="save-btn" @click="saveTask">保存</text>
			</view>
		</view>

		<!-- 任务名称输入框 -->
		<view class="task-name">
			<input v-model="taskName" placeholder="我要..." class="task-name-input" />
		</view>

		<!-- 功能区 -->
		<view class="function">
			<view class="function-sparkle">
				<i class="iconfont icon-sparkles-outline"></i>
			</view>
			<view class="function-voice" @click="startRecognize">
				<i class="iconfont icon-michuatong"></i>
			</view>
		</view>

		<!-- 语音识别 -->
		<transition name="fade">
			<view class="mask" v-if="isVisible" @click="closeRecognize" @touchmove.stop.prevent></view>
		</transition>
		<transition name="slide">
			<view class="recogniz" v-if="isVisible">
				<view style="color: #FFFFFF;">
					<text>{{ title }}</text>
				</view>
				<view class="partial">
					<text>{{ partialResult }}</text>
				</view>
				<view class="volume-container">
					<view v-for="(item, index) in volumeBars" :key="index" class="volume-bar"
						:style="{ height: item.height + 'px', backgroundColor: '#FFFFFF' }"></view>
				</view>
			</view>
		</transition>

		<!-- 最近使用 -->
		<view class="recently">
			最近使用
			<view class="recently-used">
				<view v-for="(item, index) in recently" :key="index" class="recently-used-options"
					@click="selectRecently(index)"
					:style="selectedRecentlyIndex === index ? 'background: linear-gradient(to right, #f97316, #fb913b); color: #FFFFFF' : 'background: #F1F5F9; color: #666666'">
					<i :class="item.icon" style="font-size: 24rpx;">{{ item.name }}</i>
				</view>
			</view>
		</view>

		<!-- 时间 -->
		<view class="time-details">
			时间
			<view class="chooseDate">
				<view v-for="(item, index) in ['今天', '明天', '本周', '自定义']" :key="index" class="chooseDate-date"
					:style="selectedDate === item ? 'background: linear-gradient(to right, #f97316, #fb913b); color: #FFFFFF' : 'background: #F1F5F9; color: #666666'"
					@click="selectDate(item)">
					{{ item }}
				</view>
			</view>
			<view class="setTime">
				<view class="setTime-date">
					<i class="iconfont icon-rili" style="color: #f97316; font-size: 56rpx"></i>
					<view style="width: 470rpx">
						<view v-if="selectedDate === '自定义' && multipleDates">
							{{ multipleDates.split('，').length > 2 ? multipleDates.split('，').slice(0, 2).join('，') +
								'...' : multipleDates }}
						</view>
						<view v-else>
							{{ selectedDate === "本周" ? weekRange : formatDate(currentDate) }}
						</view>
					</view>
					<view class="index">
						<wu-calendar :itemHeight='50' ref="calendar" @confirm="calendarConfirm" color="#f97316"
							:insert="false" mode="multiple" :date="[]" :startDate="today" :lunar="true"
							:clearDate="false" :maskClick="true"></wu-calendar>
						<span style="color: #f97316" @click="openCalendar">更改</span>
					</view>
				</view>
				<view class="setTime-time">
					<i class="iconfont icon-shijian" style="color: #f97316; font-size: 38rpx; margin-left: 8rpx"></i>
					<picker mode="time" :value="time" start="00:00" end="24:00" @change="bindTimeChange"
						style="width: 88%;">
						<view class="picker">
							<view class="uni-input">{{ time }}</view>
							<span style="color: #f97316;">更改</span>
						</view>
					</picker>
				</view>
			</view>
			<view class="remind">
				<i class="iconfont icon-shijian" style="color: #f97316; font-size: 38rpx"></i>
				<span style="width: 500rpx">是否提醒</span>
				<switch :checked="isRemind" @change="isRemind = $event.detail.value" color="#F97316"
					style="width: 70rpx; transform: scale(0.6)" />
			</view>
			<transition name="expand">
				<view class="setTime" v-if="isRemind">
					<view class="setTime-time">
						<i class="iconfont icon-shijian" style="color: #f97316; font-size: 38rpx; margin: 8rpx"></i>
						<picker @change="bindRemindPickerChange" :value="remindIndex" :range="beforehand"
							range-key="name" style="width: 88%;">
							<view class="picker">
								<view>
									{{ beforehand[remindIndex].name }}
								</view>
								<span style="color: #f97316">更改</span>
							</view>
						</picker>
					</view>
					<view class="setTime-date">
						<i class="iconfont icon-rili" style="color: #f97316; font-size: 56rpx"></i>
						<picker @change="bindRepeatPickerChange" :value="repeatIndex" :range="repeat" range-key="name"
							style="width: 88%">
							<view class="picker">
								<span>重复提醒</span>
								<view>
									{{ repeat[repeatIndex].name }}
								</view>
							</view>
						</picker>
					</view>
				</view>
			</transition>
		</view>
		<!-- 任务分类 -->
		<view class="task-categories">
			<text class="categories-title">分类</text>
			<scroll-view scroll-x="true">
				<view class="category-icons">
					<view class="category-icon" v-for="(color, index) in categoryColors" :key="index"
						:style="{ backgroundColor: isColor === index ? '#F1F5F9' : '' }" @click="selectCategory(index)">
						<view class="category-colors" :style="{ backgroundColor: color.color }">
							<view v-if="isColor == index" class="color"></view>
						</view>
						<view class="category-content">
							{{ color.content }}
						</view>
					</view>
					<view class="category-icon" @click="addCategory()">
						<view class="category-color" style="border: 2rpx solid #ffaa00">+
						</view>
						<view class="category-content"> 新增 </view>
					</view>
				</view>
			</scroll-view>
		</view>

		<!-- 任务矩阵 -->
		<view class="task-title">
			<i class="iconfont icon-star task-icons task-title-icons" />
			重要程度
		</view>
		<view class="task-matrix">
			<view v-for="(item, index) in matrixItems" :key="index" class="matrix-item"
				:style="{ background: item.active ? item.color : '#9E9E9E' }" @click="selectMatrixItem(index)">
				<i class="task-icons" :class="item.icon"></i>
				<text class="matrix-title">{{ item.title }}</text>
				<text class="matrix-content">{{ item.content }}</text>
				<image :src="item.url" mode="" class="task-imgs"></image>
			</view>
		</view>

		<!-- 备注输入框 -->
		<view class="task-title"><i class="iconfont icon-beizhu task-title-icons"></i>备注</view>
		<view class="task-notes">
			<textarea v-model="taskNotes" placeholder="添加备注……" class="notes-input" />
		</view>
		<view v-if="isStandby == true && standbyList.length > 0" id="taskContainer" class="task-Standby">
			<view class="radio">
				<view class="radio-title">
					<view>子任务</view>
					<view class="radio-title-add" @click="addSubtasks()">添加</view>
				</view>
				<view class="radio-title-radios" v-for="item in standbyList" :key="item.id">
					<input type="text" placeholder="请输入子任务名称" v-model="item.content" />
					<view @click="deleteSubtasks(item.id)">
						<uni-icons type="bars" size="50rpx"></uni-icons>
						<uni-icons type="trash" size="50rpx"></uni-icons>
					</view>
				</view>
				<!-- <movable-area class="move">
					<movable-view direction="vertical" @change="onChange" class="move-views">
						<view class="radio-title-radios" v-for="item in standbyList" :key="item.id">
							<radio value="radio1" />
							<input type="text" placeholder="请输入子任务名称" v-model="item.content" />
							<view @click="deleteSubtasks(item.id)">
								<uni-icons type="bars" size="50rpx"></uni-icons>
								<uni-icons type="trash" size="50rpx"></uni-icons>
							</view>
						</view>
					</movable-view>
				</movable-area> -->

			</view>
		</view>
		<view class="task-title-footer"> </view>
		<!-- 底部操作按钮 -->
		<view class="footer-buttons">
			<view class="add-to-list-btn" @click="addToList">
				<text>添加子待办</text>
			</view>
			<view class="create-now-btn" @click="createNow">
				<text>立即创建</text>
			</view>
		</view>
		<t-color-picker ref="colorPicker" :color="color" @confirm="confirm">
			<input type="text" />
		</t-color-picker>
		<uni-popup ref="popup" background-color="#fff">
			<view class="popup-root">
				<view class="popup-root-top">
					<view class="" @click="Cancel"> 取消 </view>
					<view class="" @click="sure"> 确定 </view>
				</view>
				<uni-forms ref="valiForm" :rules="rules" :model="valiFormData">
					<uni-forms-item name="Classification">
						<input type="text" v-model="valiFormData.Classification" placeholder="请输入分类名称" />
					</uni-forms-item>
				</uni-forms>
				<scroll-view scroll-x="true">
					<view class="category-icons">
						<view class="category-icon" v-for="(color, index) in categoryColor" :key="index"
							@click="selectCategorys(color, index)">
							<view class="category-colors" :style="{ backgroundColor: color }">
								<view v-if="isColors === index" class="color"></view>
							</view>
						</view>
						<view class="category-icon" @click="open()">
							<view class="categorys-color" style="border: 2rpx solid #ffaa00"><uni-icons
									type="more-filled" size="30" style="color: #f97316;"></uni-icons>

							</view>
						</view>
					</view>
				</scroll-view>
			</view>

			<!-- </view> -->
		</uni-popup>
		<!-- 日期选择器 -->
	</view>
</template>

<script>
	import wuCalendar from '@/uni_modules/wu-calendar/components/wu-calendar/wu-calendar.vue';
	import tColorPicker from "@/components/t-color-picker/t-color-picker.vue";

	export default {
		components: {
			wuCalendar,
			tColorPicker,
		},
		data() {
			return {
				// 初始的颜色
				color: {
					r: 255,
					g: 0,
					b: 0,
					a: 0.6,
				},
				// 麦克风
				isRecognizing: false, // 添加识别状态标志
				// 语音识别
				isVisible: false,
				title: '',
				text: '',
				partialResult: '...',
				result: '',
				volumeBars: Array(10).fill().map(() => ({
					height: 5
				})),
				// 提醒和重复
				remindIndex: 0,
				repeatIndex: 0,
				today: this.getTodayDate(), // 获取今天的日期(日历开始时间)
				time: new Date().getHours() + ":" + new Date().getMinutes().toString().padStart(2, "0"),
				recently: [{
						icon: "iconfont icon-michuatong",
						name: "练习演讲",
						time: "12:00",
						isRemind: false,
						remind: 0,
						repeat: 0,
						isStandby: true,
						standbyList: [{
							id: 1,
							content: "h",
						}, {
							id: 2,
							content: "e",
						}, {
							id: 3,
							content: "l",
						}, {
							id: 4,
							content: "l",
						}, {
							id: 5,
							content: "0",
						}],
						notes: "学习语文",
					},
					{
						icon: "iconfont icon-michuatong",
						name: "练习",
						time: "13:00",
						isRemind: true,
						remind: 3,
						repeat: 1,
						isStandby: false,
						notes: "学习英语",
					},
					{
						icon: "iconfont icon-michuatong",
						name: "演讲",
						time: "14:00",
						isRemind: true,
						remind: 4,
						repeat: 0,
						isStandby: false,
						notes: "学习数学",
					},
				],
				selectedRecentlyIndex: null,
				beforehand: [{
						name: "提前十分钟",
					},
					{
						name: "提前三十分钟",
					},
					{
						name: "提前一小时",
					},
					{
						name: "提前十二小时",
					},
					{
						name: "提前一天",
					},
					{
						name: "提前七天",
					},
					{
						name: "提前一个月",
					},
				],
				repeat: [{
						name: "不重复",
					},
					{
						name: "重复",
					},
				],
				// 子任务列表
				standbyList: [{
					id: 1,
					content: "",
				}, ],
				nextId: 1, // 用于生成子待办唯一ID
				// 选择后的颜色
				selectedColor: "#ffaa00",
				// 输入的标题
				Classification: "",
				isColor: 0,
				isColors: 0,
				taskName: "",
				priorityIndex: 0,
				taskNotes: "",
				categoryColor: [
					"#ffaa00",
					"#00ffff",
					"#00557f",
					"#ffaa7f",
					"#ff00ff",
					"#55007f",
					"#ff0000",
					"#ffff00",
				],
				categoryColors: [{
						color: "#4CAF50",
						content: "生活",
					},
					{
						color: "#2196F3",
						content: "健康",
					},
					{
						color: "#FF9800",
						content: "学习",
					},
					{
						color: "#9C27B0",
						content: "工作",
					},
					{
						color: "#F44336",
						content: "家庭",
					},
					{
						color: "#9E9E9E",
						content: "其他",
					},
				],
				valiFormData: {
					Classification: "",
				},
				multipleDates: '', // 用于存储不连续日期的格式化字符串
				// 日期选择器
				selectedDate: '今天',
				currentDate: new Date(),
				weekRange: '',
				// 提醒
				isRemind: false,
				// 是否显示子待办
				isStandby: false,
				// 子待办输入内容
				standby: "",
				selectedCategory: null,
				matrixItems: [{
						icon: "iconfont icon-shijiankaishishijian",
						title: "重要且紧急",
						color: `linear-gradient(to bottom right, #F97316, #FB923C)`,
						content: "需要立即处理的任务",
						active: true,
						url: "/static/task/important-1.png",
					},
					{
						icon: "iconfont icon-menling-",
						title: "重要不紧急",
						color: "linear-gradient(to bottom right, #10B981, #34D399)",
						content: "需要规划的任务",
						active: false,
						url: "/static/task/important-2.png",
					},
					{
						icon: "iconfont icon-renwu",
						title: "紧急不重要",
						color: "linear-gradient(to bottom right, #3B82F6, #60A5FA)",
						content: "可委托他人的任务",
						active: false,
						url: "/static/task/important-3.png",
					},
					{
						icon: "iconfont icon-xiala",
						title: "不重要不紧急",
						color: "linear-gradient(to bottom right,#a18cd1 , #bb8cd1 )",
						content: "可以消除的任务",
						active: false,
						url: "/static/task/important-4.png",
					},
				],
				// 表单验证
				rules: {
					// 对输入字段进行字数验证
					Classification: {
						rules: [{
								required: true,
								errorMessage: "请输入分类名称",
							},
							{
								pattern: /^\S*$/,
								errorMessage: "分类名称不能包含空格",
							},
							{
								minLength: 1,
								maxLength: 4,
								errorMessage: "分类名长度在 {minLength} 到 {maxLength} 个字符",
							},
						],
					},
				},
			};
		},

		onLoad() {
			// #ifdef APP-PLUS
			// 监听语音识别事件
			plus.speech.addEventListener('start', this.ontStart, false);
			plus.speech.addEventListener('volumeChange', this.onVolumeChange, false);
			plus.speech.addEventListener('recognizing', this.onRecognizing, false);
			plus.speech.addEventListener('recognition', this.onRecognition, false);
			plus.speech.addEventListener('end', this.onEnd, false);
			// #endif
		},

		methods: {
			goBack() {
				uni.navigateBack();
			},
			saveTask() {
				// this.saveTaskToLocal();
				uni.showToast({
					title: "已保存至草稿箱",
					icon: "success",
				});
			},
			getTodayDate() {
				const today = new Date();
				return `${today.getFullYear()}-${today.getMonth() + 1}-${today.getDate()}`;
			},
			// 最近使用
			selectRecently(index) {
				if (this.selectedRecentlyIndex === index) {
					// 点击已选中的选项，取消选择并重置所有相关值
					this.selectedRecentlyIndex = null;
					this.taskName = '';
					this.time = new Date().getHours() + ":" + new Date().getMinutes().toString().padStart(2, "0");
					this.repeatIndex = 0;
					this.isRemind = false;
					this.remindIndex = 0;
					this.isStandby = false;
					this.standbyList = [{
						id: 1,
						content: ""
					}];
					this.taskNotes = '';
				} else {
					// 正常选择新选项
					this.selectedRecentlyIndex = index;
					this.taskName = this.recently[index].name;
					this.time = this.recently[index].time;
					this.repeatIndex = this.recently[index].repeat;
					this.isRemind = this.recently[index].isRemind;
					this.remindIndex = this.recently[index].remind;
					this.isStandby = this.recently[index].isStandby;
					this.standbyList = this.recently[index].standbyList;
					this.taskNotes = this.recently[index].notes;
				}
			},
			// 选择日期
			selectDate(type) {
				this.selectedDate = type;
				const now = new Date();

				switch (type) {
					case '今天':
						this.currentDate = now;
						break;
					case '明天':
						const tomorrow = new Date(now);
						tomorrow.setDate(now.getDate() + 1);
						this.currentDate = tomorrow;
						break;
					case '本周':
						const startOfWeek = new Date(now);
						const endOfWeek = new Date(now);
						// 结束日期设置为本周日
						endOfWeek.setDate(now.getDate() + (7 - now.getDay()));
						this.weekRange = `${this.formatDate(startOfWeek)} - ${this.formatDate(endOfWeek)}`;
						break;
					case '自定义':
						this.currentDate = now;
						break;
				}
			},
			formatDate(date) {
				return `${date.getFullYear()}年${date.getMonth() + 1}月${date.getDate()}日`;
			},

			// 判断选择时间格式
			isDatesContinuous(dates) {
				dates.sort((a, b) => a - b);
				for (let i = 1; i < dates.length; i++) {
					const prevDay = new Date(dates[i - 1]);
					prevDay.setDate(prevDay.getDate() + 1);
					if (dates[i].getDate() !== prevDay.getDate()) {
						return false;
					}
				}
				return true;
			},

			// 自定义日期
			calendarConfirm(e) {
				console.log(e);
				if (e.multiple && e.multiple.length > 0) {
					this.selectedDates = e.multiple.map(date => new Date(date));
					this.selectedDate = '自定义';

					if (e.multiple.length === 1) {
						// 单个日期直接格式化显示
						this.multipleDates = this.formatDate(this.selectedDates[0]);
					} else if (this.isDatesContinuous(this.selectedDates)) {
						// 连续日期显示范围
						const startDate = this.selectedDates[0];
						const endDate = this.selectedDates[this.selectedDates.length - 1];
						this.multipleDates = `${this.formatDate(startDate)} - ${this.formatDate(endDate)}`;
					} else {
						// 不连续日期全部显示，用逗号分隔
						this.multipleDates = this.selectedDates.map(date => this.formatDate(date)).join('，');
					}
				}
			},
			// 打开日历
			openCalendar() {
				this.$refs.calendar.open();
			},

			// 取消选择颜色
			Cancel() {
				this.$refs.popup.close();
			},
			// 选择颜色
			sure() {
				this.Classification = this.valiFormData.Classification;

				this.$refs.valiForm
					.validate()
					.then((res) => {
						this.$refs.popup.close();
						this.categoryColors.push({
							color: this.selectedColor,
							content: this.valiFormData.Classification,
						});
						this.isColor = this.categoryColors.length - 1;
						this.valiFormData.Classification = ""; // 清空表单
					})
					.catch((err) => {
						console.log("表单验证失败:", err);
					});
			},
			// 颜色选择器
			open() {
				// 打开颜色选择器
				this.$refs.colorPicker.open();
			},
			confirm(e) {
				console.log("颜色选择器返回值：" + JSON.stringify(e), e);
				this.selectedColor = e.hex;
				console.log(this.selectedColor);
				this.categoryColor.push(this.selectedColor);
				this.isColors = this.categoryColor.length - 1;
			},
			// 删除子任务
			deleteSubtasks(index) {
				this.standbyList = this.standbyList.filter((item) => item.id !== index);
				if (this.standbyList.length <= 0) {
					this.isStandby = !this.isStandby;
					this.standbyList = [""]; // 修改初始化为空数组
				}
			},
			// 新增子任务
			addSubtasks() {
				this.standbyList.push({
					id: this.nextId++, // 使用唯一ID
					content: "",
				});
				console.log(this.standbyList)
				this.$nextTick(() => {
					const query = uni.createSelectorQuery().in(this);
					query
						.select("#taskContainer")
						.boundingClientRect((data) => {
							uni.pageScrollTo({
								scrollTop: data.bottom + 27 * this.standbyList.length +
									200, // 滚动到元素顶部上方100rpx
								duration: 300,
							});
						})
						.exec();
				});
			},
			// 移动效果
			onChange: function(e) {
				// this.old.x = e.detail.x
				this.old.y = e.detail.y
			},
			createNow() {
				// this.saveTaskToLocal()
				uni.switchTab({
					url: "/pages/task/task",
				}).then(() => {
					uni.showToast({
						title: "任务已创建",
						icon: "success",
					});
				});
			},
			showDatePicker() {
				this.showPicker = true;
			},
			// 新增分类
			addCategory() {
				this.$refs.popup.open("bottom");
			},
			selectCategory(index) {
				// this.selectedCategory = index
				this.isColor = index;
			},
			selectCategorys(color, index) {
				this.isColors = index;
				this.selectedColor = color;
				console.log(index, color);
			},
			selectMatrixItem(index) {
				// 遍历matrixItems数组，将所有项的active属性设置为false
				this.matrixItems.forEach((item) => {
					item.active = false;
				});

				// 将当前点击项的active属性设置为true
				this.matrixItems[index].active = true;
			},
			bindRemindPickerChange(e) {
				this.remindIndex = e.detail.value;
			},
			bindRepeatPickerChange(e) {
				this.repeatIndex = e.detail.value;
			},
			bindTimeChange: function(e) {
				this.time = e.detail.value
			},
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			// 添加子待办
			addToList() {
				this.isStandby = !this.isStandby;
				if (this.isStandby) {
					this.standbyList = [{
						id: this.nextId++, // 使用唯一ID
						content: "",
					}, ];
					this.$nextTick(() => {
						const query = uni.createSelectorQuery().in(this);
						query
							.select("#taskContainer")
							.boundingClientRect((data) => {
								uni.pageScrollTo({
									scrollTop: data.bottom + 200,
									duration: 300,
								});
							})
							.exec();
					});
				}
			},

			// 语音识别
			ontStart() {
				this.title = '...倾听中...';
				this.text = '';
				this.partialResult = '';
				this.taskName = '';
				console.log('Event: start');
			},
			onVolumeChange(e) {
				// 音频波动效果
				const volume = e.volume;
				this.volumeBars = this.volumeBars.map((_, i) => {
					// 根据音量动态调整每个bar的高度
					const baseHeight = 5 + Math.random() * 5;
					const height = baseHeight + (volume * 20) * (i % 2 === 0 ? 1 : 0.8);
					return {
						height: Math.min(30, Math.max(5, height))
					};
				});
			},
			onRecognizing(e) {
				this.partialResult = e.partialResult;
				this.taskName = this.partialResult;
				console.log('Event: recognizing');
			},
			onRecognition(e) {
				// 累积完整识别结果
				this.text += e.result;
				this.text ? (this.text += '\n') : this.text = '';
				this.result = this.text;
				this.partialResult = e.result;
				console.log('Event: recognition');
			},
			onEnd() {
				if (!this.text || this.text == '') {
					plus.nativeUI.toast('没有识别到内容');
				}
				this.result = this.text;
				this.taskName = this.result || this.partialResult;
				this.title = '未开始';
				this.partialResult = '...';
			},

			// 语音识别开启
			startRecognize() {
				console.log('startRecognize');
				this.isVisible = true;
				this.isRecognizing = true;
				// #ifdef APP-PLUS
				plus.speech.startRecognize({
					engine: 'baidu',
					lang: 'zh-cn',
					'userInterface': false,
					'continue': true
				});
				// #endif
			},
			// 关闭
			closeRecognize() {
				console.log('endRecognize');
				this.isVisible = false;
				this.isRecognizing = false;
				// #ifdef APP-PLUS
				plus.speech.stopRecognize();
				// #endif
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 0
				})
			},
		},
	};
</script>

<style scoped lang="less">
	@import url("https://at.alicdn.com/t/c/font_4945078_d8s44zrrq5.css");

	::v-deep .uni-forms-item__error {
		position: absolute;
		left: 0rpx;
	}

	.container {
		padding: 40rpx;
		min-height: 100vh;
	}

	.header {
		width: 670rpx;
		height: 170rpx;
		position: fixed;
		top: 0rpx;
		line-height: 170rpx;
		display: flex;
		justify-content: space-between;
		align-items: end;
		margin-bottom: 40rpx;
		background-color: white;
		z-index: 10;

		&-top {
			width: 670rpx;
			height: 130rpx;
		}

		&-title {
			font-size: 36rpx;
		}
	}

	.color {
		width: 30rpx;
		height: 30rpx;
		background-color: white;
		border-radius: 200rpx;
	}



	.task-title {
		margin-bottom: 20rpx;

		&-icons {
			color: #f97316;
			margin-right: 16rpx;
		}

		&-footer {
			height: 110rpx;
			width: 100%;
		}


		.back {
			font-size: 48rpx;
			padding-left: 28rpx;
			margin-left: 18rpx;

			&-icon {
				font-size: 48rpx;
			}
		}

		.header-actions {
			display: flex;
			gap: 30rpx;
			padding-right: 18rpx;
			margin-right: 16rpx;
		}
	}

	.radio {
		width: 100%;

		&-title {
			display: flex;
			justify-content: space-between;
			padding: 25rpx;
			margin-bottom: 10rpx;

			&-add {
				color: #f97316;
			}

			&-radios {
				display: flex;
				margin: 15rpx 15rpx 20rpx 15rpx;

				input {
					width: 100%;
				}
			}
		}
	}

	.task-icons {
		font-size: 36rpx;
	}

	.task-imgs {
		width: 120rpx;
		height: 12rpx;
		position: absolute;
		bottom: 2rpx;
		right: 2rpx;
	}

	.save-btn,
	.more-btn {
		color: #f97316;
		font-size: 32rpx;
	}

	.task-Standby {
		width: 100%;
		border-radius: 15rpx;
		background-color: #f1f5f9;
		padding-bottom: 30rpx;
	}

	.task-name-input {
		width: 100%;
		color: #9ca3af;
		padding-bottom: 12rpx;
		border-bottom: 2rpx solid #e5e7eb;
		font-size: 36rpx;
	}

	.function {
		width: 100%;
		height: 80rpx;
		display: flex;
		flex-direction: row-reverse;
		align-items: center;
		margin: 20rpx 0;

		&-voice {
			color: #65676a;
			width: 80rpx;
			height: 80rpx;
			margin-right: 30rpx;
			border-radius: 50%;
			text-align: center;
			line-height: 80rpx;
			background-color: #f1f5f9;
		}

		&-sparkle {
			color: #65676a;
			width: 80rpx;
			height: 80rpx;
			border-radius: 50%;
			text-align: center;
			line-height: 80rpx;
			background-color: #f1f5f9;
		}
	}

	.recently {
		color: #666666;
		font-size: 30rpx;
		margin-bottom: 20px;

		&-used {
			display: flex;
			flex-wrap: wrap;

			&-options {
				height: 30rpx;
				color: #666666;
				padding: 15rpx 25rpx;
				border-radius: 50rpx;
				background-color: #f1f5f9;
				text-align: center;
				line-height: 30rpx;
				margin: 15rpx;
			}
		}
	}

	.time-details {
		color: #666666;
		font-size: 30rpx;
		background-color: white;
		border-radius: 16rpx;
		margin-bottom: 40rpx;
	}

	.chooseDate {
		margin: 30rpx 0;
		display: flex;

		&-date {
			color: #ffffff;
			height: 60rpx;
			font-size: 24rpx;
			border-radius: 30rpx;
			padding: 0 20rpx;
			line-height: 60rpx;
			margin-right: 20rpx;
			// background-color: #F1F5F9;
			background: #F1F5F9;
		}
	}

	.setTime {
		font-size: 28rpx;
		padding: 20rpx;
		display: flex;
		flex-direction: column;
		background-color: #f1f5f9;
		border-radius: 30rpx;

		&-date {
			height: 60rpx;
			margin: 5rpx 0 5rpx 8rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
		}

		&-time {
			height: 60rpx;
			margin: 5rpx 0 5rpx 8rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
		}
	}

	.picker {
		width: 100%;
		display: flex;
		justify-content: space-between;
	}

	.remind {
		font-size: 30rpx;
		margin: 40rpx 0;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.task-categories {
		background-color: white;
		border-radius: 16rpx;
		padding: 30rpx;
		margin-bottom: 40rpx;
	}

	.categories-title {
		display: block;
		margin-bottom: 30rpx;
		color: #666;
	}

	.category-icons {
		display: flex;
		width: 100%;
	}

	.category-icon {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 20rpx;
		border-radius: 30rpx;
		margin: 0 10rpx 5rpx;
		width: 100%;
	}

	.categorys-color {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
		display: flex;
		align-items: center;
		/* line-height: 40px; */
		justify-content: center;
		font-size: 70rpx;
		color: #ffaa00;
	}

	.category-color {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 60rpx;
		color: #ffaa00;
	}

	.category-colors {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.task-matrix {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 20rpx;
		margin-bottom: 40rpx;
	}

	.matrix-item {
		padding: 30rpx;
		border-radius: 16rpx;
		color: white;
		text-align: start;
		font-weight: 500;
		position: relative;
	}

	.matrix-title {
		display: block;
		font-size: 28rpx;
		margin: 10rpx 0 0;
	}

	.matrix-content {
		font-size: 20rpx;
		margin: 0;
	}

	.task-notes {
		background-color: #f1f5f9;
		/* background-color: white; */
		border-radius: 16rpx;
		padding: 30rpx;
		margin-bottom: 40rpx;
	}

	.notes-input {
		width: 100%;
		min-height: 160rpx;
		font-size: 28rpx;
		color: #9ca3af;
	}

	.footer-buttons {
		background-color: white;
		position: fixed;
		bottom: 0rpx;
		left: 0rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 30rpx;
		padding: 10rpx 10rpx 15rpx 5rpx;
		z-index: 10;
		width: 100%;
		// height: 100rpx;
	}

	.add-to-list-btn,
	.create-now-btn {
		// flex: ;
		width: 30%;
		padding: 30rpx;
		text-align: center;
		border-radius: 9999px;
		color: white;
		font-weight: bold;
	}

	.add-to-list-btn {
		border: 2rpx solid #ff9800;
		color: #ff9800;
	}

	.create-now-btn {
		background: linear-gradient(to right, #ea580c, #fb923c);
	}

	.popup-root {
		margin: 30rpx;
		height: 400rpx;
	}

	.move {
		width: 100%;
		height: 100%;
		display: block;

		&-views {
			width: 100%;
			height: 100%;
		}
	}

	.popup-root-top {
		display: flex;
		justify-content: space-between;
		height: 76rpx;

		input {
			padding: 6rpx;
			background-color: #dddddd;
			height: 40rpx;
			width: 660rpx;
			border-radius: 26rpx;
			margin-bottom: 40rpx;
		}
	}

	.uni-forms-item__error {
		color: #ff0000;
		font-size: 24rpx;
		padding: 8rpx 0;
	}

	// 语音识别
	.mask {
		width: 100vw;
		height: 100vh;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.5);
		z-index: 999;
	}

	.recogniz {
		width: 400rpx;
		height: 200rpx;
		padding: 24rpx;
		position: fixed;
		top: 65%;
		left: 50%;
		transform: translate(-50%, -50%);
		background-color: #f97316;
		border-radius: 32rpx;
		text-align: center;
		z-index: 1000;
	}

	.partial {
		width: 100%;
		height: 80rpx;
		margin-top: 32rpx;
		font-size: 24rpx;
		color: #FFFFFF;
	}

	// 音频可视化
	.volume-container {
		display: flex;
		align-items: flex-end;
		justify-content: center;
		height: 60rpx;
		gap: 6rpx;
		margin-top: 2rpx;
	}

	.volume-bar {
		width: 6rpx;
		min-height: 10rpx;
		border-radius: 6rpx;
		transition: height 0.1s ease;
	}

	.fade-enter-active,
	.fade-leave-active {
		transition: opacity 0.3s ease;
	}

	.fade-enter,
	.fade-leave-to {
		opacity: 0;
	}

	.slide-enter-active,
	.slide-leave-active {
		transition: all 0.3s ease;
	}

	.slide-enter,
	.slide-leave-to {
		opacity: 0;
	}

	.expand-enter-active,
	.expand-leave-active {
		transition: max-height 0.3s ease;
		overflow: hidden;
	}

	.expand-enter,
	.expand-leave-to {
		max-height: 0rpx;
	}

	.expand-enter-to,
	.expand-leave {
		max-height: 600rpx;
	}
</style>