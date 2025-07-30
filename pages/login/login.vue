<template>
  <view class="app animate-page">
    <view class="app-login">
      <view class="login-title">
        <view class="login-title-img"></view>
        <text class="login-title-text">拾光</text>
        <text class="login-title-content">高效管理拾起每一刻光阴</text>
      </view>
      <form class="login-form">
        <h3 class="login-form-title">登陆账号</h3>
        <uni-forms ref="form" :modelValue="formData" :rules="rules">
          <uni-forms-item name="phone">
            <view class="form-group">
              <uni-icons type="person" size="23" class="lll"></uni-icons>
              <!-- <view class="form-group-icon-user"></view> -->
              <input
                type="number"
                placeholder="手机号/用户名"
                v-model="formData.phone"
              />
            </view>
          </uni-forms-item>
          <uni-forms-item name="password" v-show="!isCodeLogin">
            <view class="form-group">
              <view style="display: flex; align-items: center; width: 100%">
                <uni-icons type="locked" size="23" class="lll"></uni-icons>
                <input
                  type="text"
                  v-if="showPassword"
                  placeholder="请输入密码"
                  v-model="formData.password"
                />
                <input
                  type="password"
                  v-else
                  placeholder="请输入密码"
                  v-model="formData.password"
                />
                <uni-icons
                  :type="showPassword ? 'eye' : 'eye-slash'"
                  size="23"
                  class="rrr2"
                  @click="showpassword"
                  style="margin-right: 16rpx"
                ></uni-icons>
              </view>
            </view>
          </uni-forms-item>
          <uni-forms-item name="code" v-show="isCodeLogin">
            <view class="form-group">
              <view style="display: flex; align-items: center; width: 100%">
                <uni-icons type="email" size="23" class="lll"></uni-icons>
                <input
                  type="text"
                  placeholder="请输入验证码"
                  style="flex: 1"
                  v-model="formData.code"
                />
                <view
                  @click="getVerificationCode"
                  class="verification-code-btn"
                >
                  获取验证码
                </view>
              </view>
            </view>
          </uni-forms-item>
        </uni-forms>
        <button type="submit" class="submit-btn" @click="handleLogin">
          立即登录
        </button>
        <view class="form-group form-group-other">
          <label class="auto-login">
            <checkbox
              value="autoLogin"
              :checked="isAutoLogin"
              @change="onAutoLoginChange"
            />
            <text>自动登录</text>
          </label>
          <view class="forget-links">
            <text class="forget-password" @click="navigateTofindPassword"
              >忘记密码</text
            >
            <text style="margin: 0 10rpx">|</text>
            <text class="login-yanzhengma" @click="toggleLoginMethod">
              {{ isCodeLogin ? "账号密码登录" : "验证码登录" }}
            </text>
          </view>
        </view>
      </form>
    </view>
    <view class="app-bottom otherlogin">
      <text class="otherlogin-text">快速登录</text>
      <view class="otherlogin-icons">
        <view class="login-wx"></view>
        <view class="login-qq"></view>
        <view class="login-apple"></view>
      </view>
      <view class="register-link"
        ><text>没有账号？</text>
        <view class="reg-button" @click="navigateToRegister">立即注册</view>
      </view>
      <p class="footer-text">
        登录即表示您已同意<text style="color: #f97316" @click="touseragree"
          >《用户协议》</text
        >和<text style="color: #f97316" @click="toprivacy">《隐私政策》</text>
      </p>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      isCodeLogin: false,
      isAutoLogin: false,
      showPassword: false, // 新增密码显示状态
      isPrompted: false, //防止重复点击返回
      formData: {
        phone: "",
        password: "",
        code: "",
      },
      rules: {
        phone: {
          rules: [
            {
              required: true,
              errorMessage: "手机号不能为空",
            },
            {
              pattern: /^1\d{10}$/,
              errorMessage: `手机号格式不正确，需为11位数字且以1开头`,
            },
          ],
        },
        password: {
          rules: [
            {
              required: true,
              errorMessage: "密码不能为空",
            },
            {
              minLength: 6,
              maxLength: 20,
              errorMessage: "密码长度需在6到20个字符之间",
            },
          ],
        },
        code: {
          rules: [
            {
              required: true,
              errorMessage: "验证码不能为空",
            },
          ],
        },
      },
    };
  },
  methods: {
    onAutoLoginChange(e) {
      this.isAutoLogin = e.detail.value;
    },
    showpassword() {
      this.showPassword = !this.showPassword;
    },
    // 添加切换登录方式的方法
    toggleLoginMethod() {
      this.isCodeLogin = !this.isCodeLogin;
    },
    getVerificationCode() {
      // 这里可以添加获取验证码的逻辑
      console.log("获取验证码");
    },
    handleLogin() {
      uni.switchTab({
        url: "/pages/task/task",
      });
      // this.$refs.form.validate().then(res => {
      // 	console.log('表单数据信息：', res);
      // }).catch(err => {
      // 	console.error('验证失败：', err)
      // })
    },
    navigateToRegister() {
      uni.redirectTo({
        url: "/pages/login/register/register",
      });
    },
    navigateTofindPassword() {
      uni.redirectTo({
        url: "/pages/login/findPassword/findPassword",
      });
    },
    //用户协议与隐私
    touseragree() {
      uni.navigateTo({
        url: "/pages/login/webview/webviewuser",
      });
    },
    toprivacy() {
      uni.navigateTo({
        url: "/pages/login/webview/webviewprivacy",
      });
    },
  },
  // onBackPress({from}){
  // 	if(isPrompted){
  // 		return
  // 	}
  // }
};
</script>

<style>
html,
body,
uni-page-body {
  height: 100%;
  margin: 0;
}

.uni-forms-item[data-v-61dfc0d0] {
  margin-bottom: 20rpx;
}

.app {
  height: 100% !important;
  min-height: 100vw;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.app-login {
  flex: 8;
  width: 100%;
  background: white;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.login-form {
  width: 85%;
  max-width: 800rpx;
  padding: 10rpx 40rpx 0 40rpx;
}

.login-title {
  display: flex;
  flex-direction: column;
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 50rpx;

  /* margin-bottom: 20px;
	font-size: 24px;
	font-weight: 600; */
}

.login-title-img {
  width: 144rpx;
  height: 168rpx;
  background-image: url(@/static/img/titlelogo.png);
  background-size: 100% 100%;
}

.login-title-text {
  font-size: 50rpx;
  font-weight: 400;
  background: linear-gradient(to right, #ea580c, #fb923c);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  display: inline-block;
  /* 新增 */
}

.login-title-content {
  margin-top: 10rpx;
  font-size: 28rpx;
  color: #666;
  text-align: center;
  margin-bottom: 20rpx;
}

.login-form {
  margin-top: 10rpx;
}

.login-form input {
  width: 100%;
  padding: 20rpx;
  /* border: 1px solid #ccc; */
  /* border-radius: 4px; */
  font-size: 28rpx;
}

.form-group {
  display: flex;
  margin-top: 40rpx;
  background-color: #f1f5f9;
  /* border-radius: 4px; */
  align-items: center;
  justify-content: center;
  padding-left: 20rpx;
  border-radius: 12rpx;
  height: 90rpx;
}

.login-form-title {
  color: #333333;
  font-weight: 400;
}

.form-group-icon-user {
  width: 36rpx;
  height: 36rpx;
  background-image: url(@/static/icon/user.png);
  background-size: 100% 100%;
  flex-shrink: 0;
}

.form-group-icon-password {
  width: 36rpx;
  height: 36rpx;
  background-image: url(@/static/icon/password.png);
  background-size: 100% 100%;
  flex-shrink: 0;
}

.app-bottom {
  flex: 1;
  width: 100%;
  /* background: #e0e0e0; */
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 40rpx;
}

.footer-text {
  font-size: 12px;
  color: #666;
  text-align: center;
  margin-bottom: 34rpx;
}

.submit-btn {
  border: none !important;
  background: linear-gradient(to right, #ea580c, #fb923c);
  color: #ffffff;
  margin-top: 60rpx;
  width: 100%;
  border-radius: 70rpx;
  font-size: 28rpx;
  text-align: center;
  padding: 10rpx;
  transition: all 0.4s;
}

.form-group-other {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 16rpx;
  background-color: transparent !important;
  font-size: 24rpx;
  color: #666;
  margin-bottom: 0 !important;
}

.auto-login {
  display: flex;
  align-items: center;
  justify-content: center;
}

.auto-login checkbox {
  transform: scale(0.6);
  margin-top: 4rpx;
}

.otherlogin {
  flex: 2 !important;
  display: flex;
  flex-direction: column;
  margin-bottom: 16rpx;
}

/* 下边这些css 是弄出横线，然后还有那个快速登录 字体的样式 */
.otherlogin-text {
  font-size: 26rpx;
  color: #666;
  position: relative;
  padding: 0 10rpx;
  display: inline-block;
}

.otherlogin-text::before,
.otherlogin-text::after {
  content: "";
  position: absolute;
  top: 50%;
  width: 200rpx;
  height: 1rpx;
  background: #e8eaed;
}

.otherlogin-text::before {
  right: 120%;
}

.otherlogin-text::after {
  left: 120%;
}

/*  */
.otherlogin-icons {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20rpx;
  height: 100rpx;
  width: 400rpx;
}

.otherlogin-icons view {
  width: 76rpx;
  height: 76rpx;
  margin-bottom: 10rpx;
  margin-top: 12rpx;
  background-size: 100% 100%;
  border-radius: 50%;
  background-color: #f1f5f9;
}

.login-wx {
  background-image: url(@/static/icon/vx.png);
}

.login-qq {
  background-image: url(@/static/icon/qq.png);
}

.login-apple {
  background-image: url(@/static/icon/apple.png);
}

.login-wx,
.login-qq,
.login-apple {
  background-size: 40% auto !important;
  background-repeat: no-repeat;
  background-position: center center;
  transition: all 0.4s ease;
}

.login-wx:active,
.login-qq:active,
.login-apple:active {
  transform: scale(0.95);
  background-color: #e2e8f0;
}

.register-link {
  font-size: 28rpx;
  display: flex;
  margin-top: 20rpx;
  margin-bottom: 20rpx;
  align-items: center;
  justify-content: space-between;
  width: 300rpx;
}

.reg-button {
  font-size: 28rpx;
  padding: 6rpx;
  border: 2rpx solid #eb5a0e;
  border-radius: 36rpx;
  color: #eb5a0e;
  width: 140rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.4s;
}

.submit-btn:active {
  /* transform: scale(0.98); */
  opacity: 0.7;
  box-shadow: 0 2rpx 8rpx rgba(234, 88, 12, 0.2);
  transition: all 0.4s;
}

.reg-button:active {
  opacity: 0.7;
  /* transform: scale(0.98); */
  border-color: #fb923c;
  transition: all 0.4s;
}

.forget-password,
.login-yanzhengma {
  transition: color 0.3s ease;
  /* 添加过渡动画 */
  color: #666;
  /* 设置默认颜色 */
}

.forget-password:active,
.login-yanzhengma:active {
  /* 同步修正类名 */
  color: #fb923c !important;
  transform: scale(0.98);
  /* 添加点击反馈 */
}

.verification-code-btn {
  color: #ffffff;
  font-size: 26rpx;
  margin-left: 20rpx;
  white-space: nowrap;
  padding: 10rpx 22rpx;
  background: linear-gradient(to right, #ea580c, #fb923c);
  border-radius: 10rpx;
  transition: all 0.3s ease;
  margin-right: 16rpx;
}

.verification-code-btn:active {
  opacity: 0.7;
  transition: all 0.3s ease;
  color: #ffffff;
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
