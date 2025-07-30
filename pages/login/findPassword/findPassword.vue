<template>
	<view class="forgetpass animate-page">
		<view class="forgetpass-top">
			<image src="/static/logo.png" mode=""></image>
			<text style="background: linear-gradient(45deg, #EA580C, #FB923C);color: transparent;-webkit-background-clip: text;background-clip: text;margin-bottom: 50rpx;">拾光</text>
			<text style="color: #666666;font-size: 32rpx;margin: 0 auto;transform: translateY(-46rpx);">高效找回，即刻重置</text>
		</view>
		<view class="forgetpass-center">
			<text style="font-size: 46rpx; margin-left: 40rpx; margin-bottom: 15rpx; color: #333333;">找回密码</text>
			<uni-forms ref="form" :modelValue="formData" :rules="rules">
				<uni-forms-item name="phone">
					<view class="forgetpass-center-phone">
						<uni-icons type="person" size="23" class="lll"></uni-icons>
						<input type="number" placeholder="请输入手机号" v-model="formData.phone" />
					</view>
				</uni-forms-item>
				<uni-forms-item name="code">
					<view class="forgetpass-center-code">
						<uni-icons type="email" size="23" class="lll"></uni-icons>
						<input type="text" placeholder="请输入验证码" v-model="formData.code" />
						<button class="rrr">获取验证码</button>
					</view>
				</uni-forms-item>
				<uni-forms-item name="newpass">
					<view class="forgetpass-center-pass">
						<uni-icons type="locked" size="23" class="lll"></uni-icons>
						<input type="text" v-if="showPassword" placeholder="请输入新密码" v-model="formData.newpass">
						<input type="password" v-else placeholder="请输入新密码" v-model="formData.newpass">
						<uni-icons :type="showPassword ? 'eye' : 'eye-slash'" size="23" class="rrr2"
							@click="togglePassword"></uni-icons>
					</view>
				</uni-forms-item>
			</uni-forms>
			<button class="forgetpass-btn" @click="resetpass">重置密码</button>
			<view class="forgetpass-center-agreement">
				<view class="forgetpass-center-agreement-first">
					<uni-icons type="arrow-left" size="20"></uni-icons>
					<text @click="nevegitToLogin">返回登录</text>
				</view>
				<view class="forgetpass-center-agreement-second">
					重置密码即表示您已同意
					<text @click="touseragree">《用户协议》</text>和<text @click="toprivacy">《隐私政策》</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isPrompted: false, //防止重复点击返回
				showPassword: false, // 新增密码显示状态
				formData: {
					phone: '',
					code: '',
					newpass: ''
				}, //表单验证数据
				rules: {
					phone: {
						rules: [{
							required: true,
							errorMessage: '手机号不能为空'
						}, {
							pattern: /^1\d{10}$/,
							errorMessage: `手机号格式不正确，需为11位数字且以1开头`
						}]
					},
					code: {
						rules: [{
							required: true,
							errorMessage: '验证码不能为空'
						}]
					},
					newpass: {
						rules: [{
							required: true,
							errorMessage: '密码不能为空'
						}, {
							minLength: 6,
							maxLength: 20,
							errorMessage: '密码长度需在6到20个字符之间'
						}]
					}
				}
			}
		},
		methods: {
			nevegitToLogin() {
				uni.redirectTo({
					url:'/pages/login/login'
				})
			},
			togglePassword() {
				this.showPassword = !this.showPassword;
			},
			//跳转用户协议与隐私政策
			touseragree() {
				uni.navigateTo({
					url: '/pages/login/webview/webviewuser',
				})
			},
			toprivacy() {
				uni.navigateTo({
					url: '/pages/login/webview/webviewprivacy',
				})
			},
			resetpass() {
				this.$refs.form.validate().then(res => {
					console.log('表单数据信息：', res);
				}).catch(err => {
					console.error('验证失败：', err)
				})
			}
		},
		onBackPress({from}){
			if(this.isPrompted){
				return
			}
			if(from==='backbutton'){
				uni.redirectTo({
					url:'/pages/login/login',
					success:()=>{
						console.log('从找回密码返回到登录页面');
					},
					complete: () => {
						this.isPrompted = false;
					}
				})
				return true
			}
		}
	}
</script>

<style scoped lang="less">
	html,
	body,
	uni-page-body {
		height: 100%;
		margin: 0;
	}

	.uni-forms-item[data-v-61dfc0d0] {
		margin-bottom: 20rpx;
	}

	.lll {
		position: absolute;
		left: 60rpx;
		line-height: 90rpx;
	}

	.rrr {
		position: absolute;
		right: 60rpx;
		top: 50%;
		transform: translateY(-50%);
		line-height: 60rpx;
		font-size: 28rpx;
		width: 250rpx;
		padding: 0;
		background-color: #F97316;
		color: #fff;
		transition: all 0.3s ease;
	}

	.rrr:active {
		opacity: 0.7;
		transition: all 0.3s ease;
	}

	.rrr2 {
		position: absolute;
		right: 70rpx;
		top: 50%;
		transform: translateY(-50%);
	}

	.forgetpass {
		display: flex;
		flex-direction: column;
		justify-content: center;
		height:100vh !important;
		min-height: 100vw;
		width: 100%;

		&-top {
			width: 100%;
			// height: 330rpx;
			margin-top: 40rpx;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;


			text {
				font-weight: 400;
				font-size: 56rpx;
			}

			image {
				width: 168rpx;
				height: 168rpx;
				margin-bottom: 10rpx;
			}
		}


		&-center {
			display: flex;
			flex-direction: column;
			margin-top: 10rpx;

			&-phone {
				position: relative;
				margin-top: 30rpx;

				input {
					padding-left: 70rpx;
					width: 70%;
					padding-right: 10%;
					height: 90rpx;
					background-color: #F1F5F9;
					margin: 0 auto;
					border-radius: 20rpx;
				}
			}

			&-code {
				position: relative;
				margin-top: 30rpx;

				input {
					padding-left: 70rpx;
					width: 50%;
					padding-right: 30%;
					height: 90rpx;
					background-color: #F1F5F9;
					margin: 0 auto;
					border-radius: 20rpx;
				}

				button {
					width: 180rpx;
					height: 60rpx;
				}
			}

			&-pass {
				position: relative;
				margin-top: 30rpx;

				input {
					padding-left: 70rpx;
					width: 65%;
					padding-right: 15%;
					height: 90rpx;
					background-color: #F1F5F9;
					margin: 0 auto;
					border-radius: 20rpx;
				}
			}

			&-agreement {
				font-size: 20rpx;
				color: #999999;

				&-first {
					margin-top: 30rpx;
					display: flex;
					align-items: center;
					justify-content: center;

					text {
						padding-left: 10rpx;
						font-size: 30rpx;
						color: #666666;
					}
				}

				&-second {
					margin-top: 100rpx;
					display: flex;
					justify-content: center;
					align-items: center;

					text {
						color: #F97316;
					}
				}
			}
		}

		&-btn {
			margin-top: 30rpx;
			margin-bottom: 30rpx;
			width: 86%;
			height: 90rpx;
			line-height: 90rpx;
			border-radius: 50rpx;
			color: #fff;
			font-size: 32rpx;
			background: linear-gradient(45deg, #EA580C, #FB923C);
			transition: all 0.3s ease;
		}

		&-btn:active {
			opacity: 0.7;
			transition: all 0.3s ease;
		}

	}

	.animate-page {
		animation: pageSlideIn 0.6s ease;
	}

	@keyframes pageSlideIn {
		from {
			transform: translateX(30px);
			opacity: 0;
		}

		to {
			transform: translateX(0);
			opacity: 1;
		}
	}
</style>