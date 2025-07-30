<template>
	<view class="enroll animate-page">
		<view class="enroll-top">
			<image src="/static/logo.png" mode=""></image>
			<text>拾光</text>
		</view>
		<view class="enroll-center">
			<text style="font-size: 50rpx; margin-left: 40rpx; margin-bottom: 15rpx; color: #333333;">加入拾光</text>
			<text style="font-size: 32rpx; margin-left: 40rpx; color: #666666;">创建您的账号，开始高效管理每一刻</text>
			<view class="enroll-center-phone">
				<uni-forms ref="form" :modelValue="formData" :rules="rules">
					<uni-forms-item name="phone">
						<uni-icons type="person" size="23" class="lll"></uni-icons>
						<input type="number" placeholder="请输入手机号" v-model="formData.phone" />
					</uni-forms-item>
					<uni-forms-item name="code">
						<view class="enroll-center-code">
							<uni-icons type="email" size="23" class="lll"></uni-icons>
							<input type="number" placeholder="请输入验证码" v-model="formData.code" />
							<button class="rrr">获取验证码</button>
						</view>
					</uni-forms-item>
					<uni-forms-item name="firstpass">
						<view class="enroll-center-pass">
							<uni-icons type="locked" size="23" class="lll"></uni-icons>
							<input type="text" v-if="showPassword" placeholder="设置密码" v-model="formData.firstpass">
							<input type="password" v-else placeholder="设置密码" v-model="formData.firstpass">
							<uni-icons :type="showPassword ? 'eye' : 'eye-slash'" size="23" class="rrr2"
								@click="togglePassword"></uni-icons>
						</view>
					</uni-forms-item>
					<uni-forms-item name="secondpass">
						<view class="enroll-center-pass">
							<uni-icons type="locked" size="23" class="lll"></uni-icons>
							<input type="text" v-if="showConfirmPassword" placeholder="确认密码"
								v-model="formData.secondpass">
							<input type="password" v-else placeholder="确认密码" v-model="formData.secondpass">
							<uni-icons :type="showConfirmPassword ? 'eye' : 'eye-slash'" size="23" class="rrr2"
								@click="toggleConfirmPassword"></uni-icons>
						</view>
					</uni-forms-item>
				</uni-forms>
			</view>

			<button class="enroll-btn" @click="register">立即注册</button>
			<view class="enroll-center-agreement">
				<view>
					<checkbox-group>
						<label>
							<checkbox value="isagreeagreement" color="#F97316" style="transform:scale(0.7)" />
						</label>
					</checkbox-group>
				</view>
				<view class="">
					我已阅读并同意
					<text @click="touseragree">《用户协议》</text>和<text @click="toprivacy">《隐私政策》</text>
				</view>
			</view>
		</view>
		<view class="enroll-bottom">
			<view class="enroll-bottom-1">
				<view class="enroll-bottom-1-l">
				</view>
				<text>快速注册</text>
				<view class="enroll-bottom-1-r">
				</view>
			</view>
			<view class="enroll-bottom-2">
				<image src="/static/weiXin.png" mode=""></image>
				<image src="/static/QQlogin.png" mode="" style="margin: 0 50rpx 0 50rpx;"></image>
				<image src="/static/Applelogo.png" mode=""></image>
			</view>
			<view class="enroll-bottom-3">
				已有账号？
				<text @click="toLogin">登录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isagreeagreement: false,
				showPassword: false, // 新增密码显示状态
				showConfirmPassword: false, // 新增确认密码显示状态
				isPrompted: false, //防止重复点击返回
				formData: {
					phone: '',
					code: '',
					firstpass: '',
					secondpass: ''
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
					firstpass: {
						rules: [{
								required: true,
								errorMessage: '密码不能为空'
							},
							{
								minLength: 6,
								maxLength: 20,
								errorMessage: '密码长度需在6到20个字符之间'
							}
						]
					},
					secondpass: {
						rules: [{
								required: true,
								errorMessage: '密码不能为空'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									if (value !== data.firstpass) {
										callback('两次输入的密码不一致')
									}
									return true
								}
							}
						]
					}
				},
			}
		},
		methods: {
			togglePassword() {
				this.showPassword = !this.showPassword;
			},
			toggleConfirmPassword() {
				this.showConfirmPassword = !this.showConfirmPassword;
			},
			toLogin() {
				uni.redirectTo({
					url:'/pages/login/login'
				})
			},
			//跳转用户协议与隐私政策
			touseragree(){
				uni.navigateTo({
					url:'/pages/login/webview/webviewuser'
				})
			},
			toprivacy(){
				uni.navigateTo({
					url:'/pages/login/webview/webviewprivacy'
				})
			},
			//提交注册
			register() {
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
						console.log('从注册到登录页面');
					},
					complete:()=>{
						this.isPrompted=false
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

	.enroll {
		display: flex;
		flex-direction: column;
		justify-content: center;
		height: 100vh !important;
		min-height: 100vw;
		width: 100%;

		&-top {
			width: 100%;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			/* 定义背景渐变 */
			background: linear-gradient(45deg, #EA580C, #FB923C);
			/* 使文本透明，以便显示背景渐变 */
			color: transparent;
			/* 背景剪切为文本 */
			-webkit-background-clip: text;
			background-clip: text;

			text {
				font-weight: 400;
				font-size: 46rpx;
			}

			image {
				width: 116rpx;
				height: 116rpx;
				margin-bottom: 10rpx;
			}
		}

		&-center {
			display: flex;
			flex-direction: column;
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
				margin-left: 40rpx;
				margin-bottom: 30rpx;
				display: flex;
				align-items: center;
				justify-content: start;
				font-size: 20rpx;
				color: #999999;

				text {
					color: #F97316;
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
			border: none;
			background: linear-gradient(45deg, #EA580C, #FB923C);
			transition: all 0.3s ease;
		}

		&-btn:active {
			opacity: 0.7;
			transition: all 0.3s ease;
		}

		&-bottom {
			display: flex;
			flex-direction: column;
			&-1 {
				position: relative;

				text {
					color: #666666;
					font-size: 28rpx;
					position: absolute;
					left: 50%;
					top: 50%;
					transform: translate(-50%, -50%);
				}

				&-l {
					position: absolute;
					top: 50%;
					transform: translateY(-50%);
					left: 60rpx;
					width: 230rpx;
					border: 2rpx solid #E5E7EB;
				}

				&-r {
					position: absolute;
					top: 50%;
					transform: translateY(-50%);
					right: 60rpx;
					width: 230rpx;
					border: 2rpx solid #E5E7EB;
				}
			}

			&-2 {
				margin-top: 40rpx;
				margin-bottom: 20rpx;
				display: flex;
				justify-content: center;
				align-items: center;

				image {
					width: 80rpx;
					height: 80rpx;
					transition: all 0.4s;
				}

				image:active {
					opacity: 0.6;
					transition: all 0.4s;
				}
			}

			&-3 {
				margin: 0 auto;
				font-size: 30rpx;
				color: #666666;

				text {
					color: #F97316;
				}
			}
		}
	}

	/* 页面间的过度动画 */
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