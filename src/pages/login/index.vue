<template>
  <view class="login-page">
    <view class="login-card">
      <view class="title">欢迎回来</view>
      <view class="subtitle">请登录您的账号</view>
      
      <view class="form-item">
        <text class="label">用户名</text>
        <view class="input-container">
          <input
            type="text"
            v-model="username"
            placeholder="请输入用户名"
            placeholder-class="placeholder"
          />
          <text class="icon">👤</text>
        </view>
      </view>
      
      <view class="form-item">
        <text class="label">密码</text>
        <view class="input-container">
          <input
            :type="showPassword ? 'text' : 'password'"
            v-model="password"
            placeholder="请输入密码"
            placeholder-class="placeholder"
          />
          <text class="icon" @tap="togglePasswordVisibility">{{ showPassword ? '👁️‍🗨️' : '👁️' }}</text>
        </view>
      </view>
      
      <view class="options-row">
        <view class="remember">
          <checkbox 
            :checked="rememberMe" 
            @tap="rememberMe = !rememberMe"
            color="#a6c0fe"
            style="transform:scale(0.7)"
          />
          <text>记住我</text>
        </view>
        <text class="forgot" @tap="forgotPassword">忘记密码?</text>
      </view>
      
      <button class="login-btn" @tap="handleLogin">登 录</button>
      
      <view class="register-row">
        <text>还没有账号? </text>
        <text class="register-link" @tap="goToRegister">立即注册</text>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue';

const username = ref('');
const password = ref('');
const rememberMe = ref(false);
const showPassword = ref(false);

const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value;
};

const handleLogin = () => {
  if (!username.value || !password.value) {
    uni.showToast({
      title: '请填写用户名和密码',
      icon: 'none'
    });
    return;
  }
  
  // 这里添加登录逻辑
  console.log('登录信息:', {
    username: username.value,
    password: password.value,
    rememberMe: rememberMe.value
  });
  
  // 登录成功后的操作
  uni.showToast({
    title: '登录成功',
    icon: 'success',
    duration: 2000,
    success: () => {
      setTimeout(() => {
        uni.switchTab({
          url: '/pages/index/index'
        });
      }, 2000);
    }
  });
};

const forgotPassword = () => {
  uni.navigateTo({
    url: '/pages/forgot-password/index'
  });
};

const goToRegister = () => {
  uni.navigateTo({
    url: '/pages/register/index'
  });
};
</script>

<style lang="scss">
.login-page {
  width: 100%;
  min-height: 100vh;
  background: linear-gradient(135deg, #a6c0fe 0%, #c2a8fd 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40rpx;
  box-sizing: border-box;
}

.login-card {
  width: 100%;
  background-color: #ffffff;
  border-radius: 30rpx;
  padding: 50rpx 40rpx;
  box-shadow: 0 10rpx 30rpx rgba(0, 0, 0, 0.05);
  box-sizing: border-box;
}

.title {
  font-size: 40rpx;
  font-weight: bold;
  color: #333;
  text-align: center;
  margin-bottom: 20rpx;
}

.subtitle {
  font-size: 28rpx;
  color: #666;
  text-align: center;
  margin-bottom: 60rpx;
}

.form-item {
  margin-bottom: 40rpx;
}

.label {
  font-size: 28rpx;
  color: #333;
  margin-bottom: 20rpx;
  display: block;
}

.input-container {
  position: relative;
  border-radius: 12rpx;
  background-color: #f5f7fa;
  height: 90rpx;
  display: flex;
  align-items: center;
  padding: 0 30rpx;
}

input {
  flex: 1;
  height: 90rpx;
  font-size: 28rpx;
  color: #333;
}

.placeholder {
  color: #999;
}

.icon {
  font-size: 36rpx;
  color: #999;
}

.options-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 50rpx;
}

.remember {
  display: flex;
  align-items: center;
}

.remember text {
  font-size: 24rpx;
  color: #666;
  margin-left: 10rpx;
}

.forgot {
  font-size: 24rpx;
  color: #a6c0fe;
}

.login-btn {
  width: 100%;
  height: 90rpx;
  background: linear-gradient(90deg, #a6c0fe, #86a9ff);
  color: #fff;
  font-size: 32rpx;
  font-weight: 500;
  border-radius: 45rpx;
  box-shadow: 0 10rpx 20rpx rgba(166, 192, 254, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 40rpx;
  border: none;
}

.register-row {
  text-align: center;
  font-size: 26rpx;
  color: #666;
}

.register-link {
  color: #a6c0fe;
  font-weight: bold;
}
</style> 