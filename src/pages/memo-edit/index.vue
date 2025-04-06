<template>
  <view class="edit-page">
    <view class="header">
      <view class="title-container">
        <view class="back-container" @tap="goBack">
          <text class="back-icon">&lt;</text>
        </view>
        <text class="title">{{ isEdit ? '编辑备忘录' : '新建备忘录' }}</text>
      </view>
    </view>
    
    <view class="content-container">
      <view class="input-group">
        <text class="input-label">标题</text>
        <input 
          type="text" 
          v-model="memoTitle" 
          placeholder="请输入标题" 
          :focus="focusType === 'title'"
          @blur="onInputBlur"
        />
      </view>
      
      <view class="input-group">
        <text class="input-label">内容</text>
        <textarea 
          v-model="memoContent" 
          placeholder="请输入内容" 
          :focus="focusType === 'content'"
          @blur="onInputBlur"
          cursor-spacing="120"
          auto-height
        />
      </view>
      
      <view class="action-buttons">
        <button class="cancel-btn" @tap="goBack">取消</button>
        <button class="save-btn" @tap="saveMemo">保存</button>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue';

// 备忘录数据
const memoTitle = ref('');
const memoContent = ref('');
const focusType = ref('title'); // 默认先聚焦标题
const isEdit = ref(false); // 是否是编辑模式
const memoId = ref(0); // 备忘录ID

onMounted(() => {
  // 获取路由参数
  const page = getCurrentPages();
  const currentPage = page[page.length - 1];
  const id = currentPage.$page?.options?.id;
  
  // 如果有ID参数，则为编辑模式，需要加载备忘录数据
  if (id) {
    isEdit.value = true;
    memoId.value = id;
    loadMemoData(id);
  } else {
    // 新建模式，默认聚焦标题输入框
    nextTick(() => {
      focusType.value = 'title';
    });
  }
});

// 加载备忘录数据
const loadMemoData = (id) => {
  // 模拟从服务器或本地存储获取数据
  // 实际应用中需要从API或本地存储获取
  const mockData = {
    id: id,
    title: '编辑的标题',
    content: '编辑的内容...'
  };
  
  memoTitle.value = mockData.title;
  memoContent.value = mockData.content;
};

// 处理输入框失去焦点
const onInputBlur = () => {
  focusType.value = '';
};

// 保存备忘录
const saveMemo = () => {
  // 验证标题
  if (!memoTitle.value.trim()) {
    uni.showToast({
      title: '标题不能为空',
      icon: 'none'
    });
    return;
  }
  
  // 构建备忘录数据
  const memo = {
    id: isEdit.value ? memoId.value : Date.now(), // 编辑模式使用原ID，新建模式生成新ID
    title: memoTitle.value,
    content: memoContent.value,
    time: getCurrentTime()
  };
  
  // 模拟保存数据
  // 实际应用中需要调用API或保存到本地存储
  console.log('保存备忘录:', memo);
  
  uni.showToast({
    title: isEdit.value ? '更新成功' : '添加成功',
    icon: 'success',
    duration: 2000,
    success: () => {
      setTimeout(() => {
        goBack();
      }, 1500);
    }
  });
};

// 获取当前时间
const getCurrentTime = () => {
  const now = new Date();
  const year = now.getFullYear();
  const month = now.getMonth() + 1;
  const day = now.getDate();
  const hour = now.getHours();
  const minute = now.getMinutes();
  
  return `${year}年${month}月${day}日 ${hour}:${minute < 10 ? '0' + minute : minute}`;
};

// 返回上一页
const goBack = () => {
  uni.navigateBack();
};
</script>

<style lang="scss">
.edit-page {
  width: 100%;
  min-height: 100vh;
  background: linear-gradient(135deg, #a6c0fe 0%, #c2a8fd 100%);
  position: relative;
}

.header {
  background: linear-gradient(135deg, #a6c0fe 0%, #c2a8fd 100%);
}

.title-container {
  height: 120rpx;
  display: flex;
  align-items: center;
  position: relative;
  padding: 0 30rpx;
}

.back-container {
  position: absolute;
  left: 30rpx;
}

.back-icon {
  font-size: 40rpx;
  color: white;
}

.title {
  flex: 1;
  text-align: center;
  font-size: 44rpx;
  font-weight: bold;
  color: #fff;
}

.content-container {
  padding: 40rpx;
  background-color: #fff;
  border-radius: 30rpx;
  margin: 30rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.08);
}

.input-group {
  margin-bottom: 30rpx;
}

.input-label {
  font-size: 30rpx;
  color: #666;
  margin-bottom: 16rpx;
  display: block;
}

input {
  width: 100%;
  height: 90rpx;
  font-size: 36rpx;
  color: #333;
  border-bottom: 2rpx solid #eee;
  padding: 10rpx 0;
}

textarea {
  width: 100%;
  min-height: 400rpx;
  font-size: 32rpx;
  color: #333;
  line-height: 1.6;
  border: 2rpx solid #eee;
  border-radius: 12rpx;
  padding: 20rpx;
  box-sizing: border-box;
}

.action-buttons {
  display: flex;
  justify-content: space-between;
  margin-top: 40rpx;
}

.cancel-btn, .save-btn {
  width: 45%;
  height: 90rpx;
  border-radius: 45rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32rpx;
}

.cancel-btn {
  background-color: #f0f0f0;
  color: #666;
}

.save-btn {
  background: linear-gradient(90deg, #5c7cfa, #845ef7);
  color: #fff;
}
</style> 