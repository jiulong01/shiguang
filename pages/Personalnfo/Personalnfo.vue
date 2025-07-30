<template>
	<view class="personal-info-page">
		<!-- 导航栏 -->
		<view class="navbar" @click="toBack">
			<uni-icons type="left" size="50rpx"></uni-icons>
			<text class="page-title">个人信息</text>
		</view>

		<!-- 个人信息卡片 -->
		<view class="info-card">
			<view class="avatar-container" @click="uploadAvatar">
				<image src="@/static/task/task-avatar.png" alt="" class="avatar" mode="aspectFill" />
				<view class="edit-avatar-btn">
					<image src="@/static/info/Camera.png" alt="" mode="aspectFit" />
				</view>
			</view>
			<view class="user-basic">
				<view class="username">
					<span>{{ userInfo.name }}</span>
					<img src="@/static/vip/vipLogo.png" class="" alt="" />
				</view>
				<view class="user-id">ID: {{ userInfo.id }}</view>
			</view>
		</view>

		<!-- 基础信息 -->
		<view class="section">
			<text class="section-title">基础信息</text>
			<text class="section-right" @click="changeInfo">更改</text>
		</view>
		<view class="info-section" @click="handleClickOutside">
			<view class="info-list">
				<view class="info-item" v-for="(item, index) in basicInfoItems" :key="index">
					<text class="label">{{ item.label }}</text>
					<view class="value-wrapper" @click.stop="startEditing(item.field, index)">
						<text v-if="!isEditing" class="value">{{
              userInfo.basicInfo[item.field]}}</text>
						<input v-else v-model="tempValue[item.field]" @blur="saveEdit(item.field)"
							@keyup.enter="saveEdit(item.field)" class="edit-input" :type="item.type || 'text'"
							:ref="`editInput${index}`"
							:style="isEditing[item.field] ? { borderBottom: '1px solid #FF7A00' }: {}" />
					</view>
				</view>
			</view>
		</view>
		<!-- 模式切换 -->
		<text class="section-title">模式切换</text>
		<view class="info-section">
			<view class="info-list account-list">
				<view class="info-item account-item">
					<text class="label">切换模式</text>
					<view class="value-wrapper account-value">
						<switch style="transform: scale(0.7)" color="#FF7A00" :checked="isDarkMode"
							@change="switch1Change" />
					</view>
				</view>
			</view>
		</view>
		<!-- 账号关联 -->
		<text class="section-title">账号关联</text>
		<view class="info-section">
			<view class="info-list account-list">
				<view class="info-item account-item">
					<view class="icon-yj">
						<i class="iconfont icon-youjian"></i>
					</view>
					<text class="label">邮箱</text>
					<view class="value-wrapper account-value">
						<text class="value">{{ userInfo.accounts.email }}</text>
						<text class="verified">已验证</text>
						<uni-icons type="right" size="45rpx"></uni-icons>
					</view>
				</view>
				<view class="info-item account-item">
					<view class="icon-vx">
						<i class="iconfont icon-weixin"></i>
					</view>
					<text class="label">微信</text>
					<view class="value-wrapper account-value">
						<text class="value">已关联</text>
						<uni-icons type="right" size="45rpx"></uni-icons>
					</view>
				</view>
				<view class="info-item account-item">
					<view class="icon-vb">
						<i class="iconfont icon-xinlangweibo"></i>
					</view>
					<text class="label">微博</text>
					<view class="value-wrapper account-value">
						<text class="value">未关联</text>
						<uni-icons type="right" size="45rpx"></uni-icons>
					</view>
				</view>
				<view class="info-item account-item">
					<view class="icon-qq">
						<i class="iconfont icon-QQ"></i>
					</view>
					<text class="label">QQ</text>
					<view class="value-wrapper account-value">
						<text class="value">已关联</text>
						<uni-icons type="right" size="45rpx"></uni-icons>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isDarkMode: false,
				userInfo: {
					name: "Alex Wang",
					id: "12345678",
					basicInfo: {
						name: "Alex",
						gender: "男",
						birthday: "1990-01-01",
						phone: "138****1234",
					},
					accounts: {
						email: "alex***@gmail.com",
						wechat: "已关联",
						weibo: "未关联",
						qq: "已关联",
					},
				},
				isEditing: false, // 编辑状态管理
				tempValue: {
					name: "Alex Wang",
					gender: "男",
					birthday: "1990-01-01",
					phone: "138****1234",
					id: "12345678",
				}, // 临时输入值
				basicInfoItems: [{
						label: "姓名",
						field: "name",
						type: "text",
					},
					{
						label: "性别",
						field: "gender",
						type: "text",
					},
					{
						label: "生日",
						field: "birthday",
						type: "date",
					},
					{
						label: "手机号码",
						field: "phone",
						type: "tel",
					},
				],
			};
		},
		methods: {
			toBack() {
				uni.navigateBack();
			},
			// 这里可以添加编辑信息、上传头像等交互方法
			editInfo() {
				// 编辑信息逻辑
			},
			uploadAvatar() {
				// 上传头像逻辑
			},

			startEditing(field, index) {
				this.isEditing[field] = true;
				this.tempValue[field] = this.userInfo.basicInfo[field];
				this.$nextTick(() => {
					const input = this.$refs[`editInput${index}`];
					if (input) input.focus();
				});
			},
			switch1Change() {
				// 切换模式功能
			},
			changeInfo() {
				this.isEditing = !this.isEditing
			},
			saveEdit(field) {
				this.userInfo.basicInfo[field] = this.tempValue[field];
				this.isEditing[field] = false;
				// Add API call here to save to server if needed
			},
			handleClickOutside() {
				Object.keys(this.isEditing).forEach((field) => {
					if (this.isEditing[field]) {
						this.saveEdit(field);
					}
				});
			},
		},
	};
</script>

<style scoped lang="less">
	@import url("//at.alicdn.com/t/c/font_4986921_kdgybha0sf8.css");

	/* 基础样式 */
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		font-family: "Arial", sans-serif;
	}

	.personal-info-page {
		background-color: #ffffff;
		min-height: 100vh;
		padding-bottom: 60px;
	}

	/* 导航栏 */
	.navbar {
		margin: 45rpx 0rpx 0rpx;
		display: flex;
		align-items: center;
		padding: 15px 20px 5rpx 14rpx;
		background-color: #ffffff;
	}

	.back-btn {
		background: none;
		border: none;
		font-size: 20px;
		margin-right: 15px;
		cursor: pointer;
		color: #333333;
	}

	.page-title {
		font-size: 18px;
		font-weight: 600;
		color: #333333;
		// flex: 1;
		// text-align: center;
	}

	/* 个人信息卡片 */
	.info-card {
		background: linear-gradient(to right, #EA580C, #FB923C);
		color: white;
		margin: 25rpx;
		padding: 30rpx 30rpx;
		border-radius: 40rpx;
		display: flex;
		align-items: center;
	}

	.avatar-container {
		position: relative;
		margin-right: 20px;
	}

	.avatar {
		width: 120rpx;
		height: 120rpx;
		border-radius: 50%;
		/* object-fit: cover; */
	}

	.edit-avatar-btn {
		position: absolute;
		bottom: 0;
		right: 0rpx;
		background-color: #ff7a00;
		border: none;
		width: 25px;
		height: 25px;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		color: #ff7a00;
		font-size: 12px;
	}

	.user-basic {
		flex: 1;
	}

	.username {
		display: flex;
		align-items: center;
		font-size: 22px;
		font-weight: bold;
		margin-bottom: 5px;
		gap: 10rpx;
	}

	.vip-icon {
		margin-left: 8px;
		font-size: 16px;
	}

	.user-id {
		font-size: 14px;
		opacity: 0.9;
	}

	/* 信息区域通用样式 */
	.info-section {
		margin: 0rpx 30rpx 30rpx;
		background-color: white;
		border-radius: 15px;
		background-color: #f1f5f9;
		overflow: hidden;
	}

	.section {
		display: flex;
		justify-content: space-between;

		&-title {
			font-size: 16px;
			font-weight: 600;
			color: #666666;
			padding: 5px 20px;
		}

		&-right {
			font-size: 28rpx;
			text-align: center;
			color: #666666;
			padding: 5px 28px;
		}
	}

	.info-list {
		list-style: none;
	}

	.info-item {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 15px 20px;
		border-bottom: 1px solid #f0f0f0;
	}

	.info-item:last-child {
		border-bottom: none;
	}

	.label {
		font-size: 15px;
		color: #333333;
	}

	.value-wrapper {
		display: flex;
		align-items: center;
	}

	.value {
		font-size: 15px;
		color: #666666;
		text-align: right;
	}

	.edit-btn {
		background: none;
		border: none;
		color: #999999;
		margin-left: 10px;
		cursor: pointer;
	}

	/* 账号关联特有样式 */
	.account-list .info-item {
		padding-right: 15px;
	}

	.account-value {
		justify-content: flex-end;
		flex: 1;
	}

	.verified {
		font-size: 13px;
		color: #00c851;
		margin-left: 8px;
		padding: 2px 5px;
		background-color: #f0fff4;
		border-radius: 4px;
	}

	.arrow-icon {
		color: #999999;
		margin-left: 15px;
		font-size: 16px;
	}

	.icon {
		&-yj {
			margin-right: 10rpx;
			width: 50rpx;
			height: 50rpx;
			border-radius: 100rpx;
			text-align: center;
			line-height: 50rpx;
			color: #f97316;
			background-color: rgba(249, 115, 22, 0.125);
		}

		&-vx {
			margin-right: 10rpx;
			border-radius: 100rpx;
			text-align: center;
			line-height: 50rpx;
			width: 50rpx;
			height: 50rpx;
			color: #07c160;
			background-color: rgba(7, 194, 96, 0.125);
		}

		&-vb {
			margin-right: 10rpx;
			border-radius: 100rpx;
			text-align: center;
			line-height: 50rpx;
			width: 50rpx;
			height: 50rpx;
			color: #e6162d;
			background-color: rgba(230, 22, 45, 0.125);
		}

		&-qq {
			margin-right: 10rpx;
			border-radius: 100rpx;
			text-align: center;
			line-height: 50rpx;
			width: 50rpx;
			height: 50rpx;
			color: #12b7f5;
			background-color: rgba(18, 183, 245, 0.125);
		}
	}

	/* 底部头像区域 */
	.bottom-avatar-section {
		position: fixed;
		bottom: 20px;
		left: 50%;
		transform: translateX(-50%);
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.bottom-avatar-section .avatar-container {
		position: relative;
	}

	.bottom-avatar-section .avatar {
		width: 60px;
		height: 60px;
	}

	.camera-btn {
		position: absolute;
		bottom: 0;
		right: 0;
		background-color: #ff7a00;
		border: none;
		width: 22px;
		height: 22px;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		color: white;
		font-size: 10px;
		border: 2px solid white;
	}
</style>

.edit-input { background: transparent; border: none; text-align: right;
font-size: 15px; color: #666666; padding: 2px 5px; width: auto; min-width:
100px; &:focus { outline: none; border-bottom: 1px solid #FF7A00; } }