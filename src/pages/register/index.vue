<template>
  <view class="register-page">
    <view class="register-card">
      <view class="title">创建账号</view>
      <view class="subtitle">请填写以下信息完成注册</view>
      
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
      
      <view class="form-item">
        <text class="label">确认密码</text>
        <view class="input-container">
          <input
            :type="showConfirmPassword ? 'text' : 'password'"
            v-model="confirmPassword"
            placeholder="请再次输入密码"
            placeholder-class="placeholder"
          />
          <text class="icon" @tap="toggleConfirmPasswordVisibility">{{ showConfirmPassword ? '👁️‍🗨️' : '👁️' }}</text>
        </view>
      </view>
      
      <view class="terms-row">
        <checkbox 
          :checked="agreeTerms" 
          @tap="agreeTerms = !agreeTerms"
          color="#a6c0fe"
          style="transform:scale(0.7)"
        />
        <text>我已阅读并同意<text class="terms-link" @tap="showTerms">服务条款</text></text>
      </view>
      
      <button class="register-btn" @tap="handleRegister">注 册</button>
      
      <view class="login-row">
        <text>已有账号? </text>
        <text class="login-link" @tap="goToLogin">返回登录</text>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue';

const username = ref('');
const password = ref('');
const confirmPassword = ref('');
const agreeTerms = ref(false);
const showPassword = ref(false);
const showConfirmPassword = ref(false);

const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value;
};

const toggleConfirmPasswordVisibility = () => {
  showConfirmPassword.value = !showConfirmPassword.value;
};

const handleRegister = () => {
  // 表单验证
  if (!username.value || !password.value || !confirmPassword.value) {
    uni.showToast({
      title: '请填写完整注册信息',
      icon: 'none'
    });
    return;
  }
  
  if (password.value !== confirmPassword.value) {
    uni.showToast({
      title: '两次输入的密码不一致',
      icon: 'none'
    });
    return;
  }
  
  if (!agreeTerms.value) {
    uni.showToast({
      title: '请同意服务条款',
      icon: 'none'
    });
    return;
  }
  
  // 这里添加注册逻辑
  console.log('注册信息:', {
    username: username.value,
    password: password.value
  });
  
  // 注册成功后的操作
  uni.showToast({
    title: '注册成功',
    icon: 'success',
    duration: 2000,
    success: () => {
      setTimeout(() => {
        goToLogin();
      }, 2000);
    }
  });
};

const showTerms = () => {
  uni.showModal({
    title: '服务条款',
    content: '这是服务条款内容...',
    showCancel: false
  });
};

const goToLogin = () => {
  uni.navigateTo({
    url: '/pages/login/index'
  });
};
</script>

<style lang="scss">
.register-page {
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

.register-card {
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

.terms-row {
  display: flex;
  align-items: center;
  margin-bottom: 50rpx;
  font-size: 24rpx;
  color: #666;
}

.terms-link {
  color: #a6c0fe;
  margin-left: 4rpx;
}

.register-btn {
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

.login-row {
  text-align: center;
  font-size: 26rpx;
  color: #666;
}

.login-link {
  color: #a6c0fe;
  font-weight: bold;
}
</style> 