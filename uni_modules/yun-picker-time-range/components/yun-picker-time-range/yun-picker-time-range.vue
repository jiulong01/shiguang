<template>
	<view class="picker-box">
		<view class="picker-head">
			<text @click="cancle">取消</text>
			<view class="picker-title">
				<text>时间区间选择</text>
			</view>
			<text @click="confirm">确定</text>
		</view>
		<view class="picker-range">
			<view class="picker-range-item" :style="getStyle('start')" @click="selectItem('start')">
				<text>{{ start_time }}</text>
			</view>
			<text class="picker-range-text">至</text>
			<view class="picker-range-item" :style="getStyle('end')" @click="selectItem('end')">
				<text>{{ end_time }}</text>
			</view>
		</view>
		<picker-view
			:indicator-style="indicatorStyle"
			:value="type == 'start' ? start_time_val : end_time_val"
			@change="bindChange"
			class="picker-view"
		>
			<picker-view-column>
				<view class="picker-item" v-for="(item, index) in hour" :key="index">{{ item }}时</view>
			</picker-view-column>
			<picker-view-column>
				<view class="picker-item" v-for="(item, index) in minute" :key="index">{{ item }}分</view>
			</picker-view-column>
		</picker-view>
	</view>
</template>

<script>
	export default {
		name: 'yun-picker-time-range',
		props: {
			color: {
				type: String,
				default: '#3c9cff',
			},
		},
		emits: ['cancle', 'confirm'],
		data() {
			let hour = []
			let minute = []
			for (let i = 0; i <= 23; i++) {
				//不足两位时前面填充0
				if (i < 10) {
					i = '0' + i
				}
				hour.push(i)
			}
			for (let i = 0; i <= 59; i++) {
				//不足两位时前面填充0
				if (i < 10) {
					i = '0' + i
				}
				minute.push(i)
			}
			return {
				type: 'start',
				hour: hour,
				minute: minute,
				indicatorStyle: `height: 50px;`,
				start_time: '00:00',
				start_time_val: [0, 0],
				end_time: '23:59',
				end_time_val: [23, 59],
			}
		},
		methods: {
			bindChange(e) {
				if (this.type == 'start') {
					this.start_time_val = e.detail.value
					this.start_time = this.hour[e.detail.value[0]] + ':' + this.minute[e.detail.value[1]]
				} else {
					this.end_time_val = e.detail.value
					this.end_time = this.hour[e.detail.value[0]] + ':' + this.minute[e.detail.value[1]]
				}
			},
			selectItem(type) {
				this.type = type
			},
			getStyle(type) {
				if (this.type == type) {
					return {
						color: this.color,
						fontWeight: 'bold',
					}
				} else {
					return {}
				}
			},
			reset() {
				this.type = 'start'
				this.start_time = '00:00'
				this.start_time_val = [0, 0]
				this.end_time = '23:59'
				this.end_time_val = [23, 59]
			},
			cancle() {
				this.$emit('cancle')
				this.reset()
			},
			confirm() {
				this.$emit('confirm', {
					start_time: this.start_time,
					start_time_val: this.start_time_val,
					end_time: this.end_time,
					end_time_val: this.end_time_val,
				})
				this.reset()
			},
		},
	}
</script>

<style lang="scss" scoped>
	.picker-box {
		.picker-head {
			display: flex;
			flex-direction: row;
			align-items: center;
			padding: 20rpx;
			border-bottom: 1rpx solid #e6e6e6;
			font-size: 30rpx;

			.picker-title {
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: center;
				flex: 1;
			}
		}
		.picker-view {
			width: 100%;
			height: 400rpx;
			margin-top: 20rpx;
		}
		.picker-item {
			line-height: 100rpx;
			text-align: center;
		}
		.picker-range {
			display: flex;
			flex-direction: row;
			align-items: center;
			padding: 20rpx;
			border-bottom: 1rpx solid #e6e6e6;
			.picker-range-item {
				flex: 1;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: center;
			}
			.picker-range-text {
				font-size: 30rpx;
			}
		}
	}
</style>
