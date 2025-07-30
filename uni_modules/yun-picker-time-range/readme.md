# 时间段选择组件

用于选择营业时间段等时间段区间选择场景，本插件只做了视图部分，可以自行选择你组件库中的弹窗进行弹出使用，避免插件内部冗余，也方便集成任意组件库弹窗使用！

# 使用组件

```
	<yun-picker-time-range @cancle="cancle" @confirm="confirm"></yun-picker-time-range>
```

# 弹窗使用案列

```
<template>
	<uv-popup ref="popup" mode="bottom" round="20rpx">
		<yun-picker-time-range @cancle="cancle" @confirm="confirm"></yun-picker-time-range>
	</uv-popup>
</template>

<script>
export default {
	name: 'yun-select-time-range',
	props: {},
	emits: ['cancle', 'confirm'],
	data() {
		return {};
	},
	methods: {
		show() {
			this.$refs.popup.open();
		},
		close() {
			this.$refs.popup.close();
		},
		cancle() {
			this.close();
			this.$emit('cancle');
		},
		confirm(e) {
			this.$emit('confirm', e);
			this.close();
		}
	}
};
</script>

<style lang="scss" scoped></style>

```
## 🔊 同道中人交流群

<table>
<thead>
<tr>
<th>QQ交流2群：971617215</th>
<th>QQ交流1群：971617215(已满)</th>
<th>作者微信：ZHT131572</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center"><img src="https://wb.ybym.top/uploads/20240926/8d0615a21db9dcb12a9bc309c2ecc07a.jpg" width="120"></td>
<td align="center"><img src="https://wb.ybym.top/uploads/20240926/bd09400c0caf7624fcf4207781ce602f.jpg" width="120"></td>
<td align="center"><img src="https://wb.ybym.top/uploads/20240926/c339c352281a51bf5b42deb3225cb593.png" width="120"></td>
</tr>
</tbody>
</table>
