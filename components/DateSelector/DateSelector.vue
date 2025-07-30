<template>
	<view class="calendar">
		<view class="calendar-wrapper">
			<!-- 		<view class="calendar-month">
				{{curMonth}}月
			</view> -->
			<view class="calendar-daylist">
				<!-- 添加点击事件和选中状态类 -->
				<view v-for="(item, index) in list" @click="chooseDay(index)"
					:class="['calendar-daylist-item', { active: index === active}]" :key="index">
					<view class="calendar-daylist-day">
						{{item | filterDay}}
					</view>
					<view class="calendar-daylist-week">
						{{item | filterWeek}}
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		getNextDay,
		getBeforeDay,
		getCurMonthDayNum,
		getElseMonthDayNum,
		isSameDay,
		timeFormat,
		geRecentDay
	} from "./index.js"
	export default {
		name: "niu-calendar",
		props: {
			/**
			 * 是否立即触发change
			 */
			rightnow: {
				type: Boolean,
				default: true
			},
			// 时间范围
			day: {
				type: Number,
				default: 0.6 * 7
			}
		},
		filters: {
			filterWeek: function(time) {
				let t = new Date(time)
				let week = t.getDay()
				let cur = ['周日', '周一', '周二', '周三', '周四', '周五', '周六'][week]
				return cur ? cur : ''
			},
			filterYear: function(time) {
				let t = new Date(time)
				let year = t.getFullYear()
				return year ? year : ''
			},
			filterDay: function(time) {
				let t = new Date(time)
				let day = t.getDate()
				return day ? day : ''
			},
		},
		computed: {
			curYear() {
				if (this.list[this.active]) {
					let date = new Date(this.list[this.active])
					let year = date.getFullYear()
					return year
				}
				return ''
			},
			curMonth() {
				if (this.list[this.active]) {
					let date = new Date(this.list[this.active])
					let month = date.getMonth() + 1
					return month
				}
				return ''
			}
		},
		data() {
			return {
				active: -1,
				todayIndex: -1,
				list: []
			};
		},
		watch: {
			day: {
				handler(newValue, oldValue) {
					// console.time("create")
					const day = geRecentDay(this.day);
					// console.timeEnd("create")
					this.active = day.index;
					this.todayIndex = day.index;
					this.list = day.list;
					if (this.rightnow) {
						this.$emit("change", this.list[this.active])
					}
				},
				immediate: true
			}
		},
		methods: {
			chooseDay(index) {
				console.log(index)
				console.log(this.todayIndex)
				// if (this.todayIndex > index) {
				// 	return;
				// }
				if (this.active === index) {
					return;
				}
				this.active = index
				this.$emit("change", this.list[this.active])
			}
		}
	}
</script>

<style lang="scss" scoped>
	.calendar {
		.calendar-wrapper {
			height: 120rpx;
			overflow: hidden;
			box-shadow: 0px 5px 5px 0px rgba(87, 139, 255, 0.1);
			display: flex;
			align-items: center;

			.calendar-year {
				font-size: 40rpx;
				font-weight: 500;
				color: #232323;
			}

			.calendar-month {
				height: 80rpx;
				width: 120rpx;
				text-align: center;
				line-height: 80rpx;
				margin: 20rpx 0;
				border-right: 2px solid #578BFF;
				font-size: 40rpx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #232323;
			}

			.calendar-daylist {
				overflow: auto;
				flex: 1;
				width: 0;
				white-space: nowrap;
				height: 100%;

				.calendar-daylist-item {
					height: 100%;
					// width: ;
					display: inline-flex;
					flex-direction: column;
					justify-content: center;
					align-items: center;
					padding: 0 18rpx;
					border-radius: 48rpx;
					color: #888;
					margin: 0rpx 11.8rpx;
					background-color: #F1F5F9;
					overflow: hidden;

					&.disable {

						.calendar-daylist-week,
						.calendar-daylist-day {
							color: #909090;
						}

					}

					.calendar-daylist-week {
						font-size: 20rpx;
						font-weight: 400;

					}

					.calendar-daylist-day {
						margin-top: 10rpx;
						font-size: 28rpx;
						font-weight: 400;
					}

					&.active {
						background: linear-gradient(to right, #EA580C, #FB923C) !important;
						color: white !important;
					}
				}
			}
		}
	}
</style>