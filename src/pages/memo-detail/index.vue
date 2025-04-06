<template>
  <view class="detail-page">
    <view class="header">
      <view class="title-container">
        <view class="back-container" @tap="goBack">
          <text class="back-icon">&lt;</text>
        </view>
        <text class="title">备忘详情</text>
        <view class="delete-container" @tap="showDeleteConfirm">
          <text class="delete-icon">🗑️</text>
        </view>
      </view>
    </view>
    
    <view class="content-container">
      <view class="memo-title" v-if="!isEditing" @tap="startEditing('title')">
        <text>{{memoDetail.title}}</text>
        <text class="edit-hint">点击编辑</text>
      </view>
      <view class="memo-title-edit" v-else>
        <input 
          type="text" 
          v-model="editTitle" 
          placeholder="请输入标题" 
          :focus="focusType === 'title'"
          @blur="onInputBlur"
          @confirm="saveEdit"
        />
      </view>
      
      <view class="memo-time">
        <text>{{memoDetail.time}}</text>
      </view>
      
      <view class="memo-content" v-if="!isEditing" @tap="startEditing('content')">
        <text>{{memoDetail.content}}</text>
        <view class="edit-hint-content">点击编辑</view>
      </view>
      <view class="memo-content-edit" v-else>
        <textarea 
          v-model="editContent" 
          placeholder="请输入内容" 
          :focus="focusType === 'content'"
          @blur="onInputBlur"
          cursor-spacing="120"
          auto-height
        />
      </view>
      
      <view class="edit-buttons" v-if="isEditing">
        <button class="cancel-btn" @tap="cancelEdit">取消</button>
        <button class="save-btn" @tap="saveEdit">保存</button>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue';

const memoDetail = ref({
  id: 0,
  title: '',
  content: '',
  time: ''
});

// 编辑状态
const isEditing = ref(false);
const editTitle = ref('');
const editContent = ref('');
const focusType = ref(''); // 用于控制哪个输入框获取焦点

onMounted(() => {
  const page = getCurrentPages();
  const currentPage = page[page.length - 1];
  const id = currentPage.$page?.options?.id || 1;
  
  // 加载备忘录详情数据
  loadMemoDetail(id);
});

// 加载备忘录详情
const loadMemoDetail = (id) => {
  // 模拟从服务器获取数据
  // 实际应用中应从API或本地存储获取
  const mockData = {
    id: id,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。\n\n内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容。',
    time: '2023年10月25日 15:30'
  };
  
  memoDetail.value = mockData;
};

// 返回上一页
const goBack = () => {
  uni.navigateBack();
};

// 开始编辑
const startEditing = (type) => {
  editTitle.value = memoDetail.value.title;
  editContent.value = memoDetail.value.content;
  isEditing.value = true;
  
  // 设置焦点类型，延迟执行确保界面已更新
  nextTick(() => {
    focusType.value = type;
    
    // 使用JavaScript直接设置焦点，增强可靠性
    if (type === 'content') {
      setTimeout(() => {
        const textarea = document.querySelector('.memo-content-edit textarea');
        if (textarea) {
          textarea.focus();
        }
      }, 100);
    }
  });
};

// 处理输入框失去焦点
const onInputBlur = () => {
  // 仅清除焦点类型，不影响编辑状态
  focusType.value = '';
};

// 取消编辑
const cancelEdit = () => {
  isEditing.value = false;
  focusType.value = '';
};

// 保存编辑
const saveEdit = () => {
  if (!editTitle.value.trim()) {
    uni.showToast({
      title: '标题不能为空',
      icon: 'none'
    });
    return;
  }
  
  // 更新备忘录数据
  memoDetail.value.title = editTitle.value;
  memoDetail.value.content = editContent.value;
  memoDetail.value.time = getCurrentTime();
  
  // 保存到服务器或本地存储
  // 这里仅做演示，实际应用中需要实现数据持久化
  
  uni.showToast({
    title: '保存成功',
    icon: 'success'
  });
  
  isEditing.value = false;
  focusType.value = '';
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

// 显示删除确认
const showDeleteConfirm = () => {
  uni.showModal({
    title: '确认删除',
    content: '确定要删除此备忘录吗？',
    success: (res) => {
      if (res.confirm) {
        deleteMemo();
      }
    }
  });
};

// 删除备忘录
const deleteMemo = () => {
  // 实际应用中应调用API或更新本地存储
  uni.showToast({
    title: '删除成功',
    icon: 'success',
    duration: 2000,
    success: () => {
      setTimeout(() => {
        uni.navigateBack();
      }, 2000);
    }
  });
};
</script>

<style lang="scss">
.detail-page {
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

.delete-container {
  position: absolute;
  right: 30rpx;
}

.delete-icon {
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

.memo-title {
  font-size: 40rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 20rpx;
  position: relative;
  padding: 10rpx;
  border-radius: 8rpx;
}

.memo-title:active {
  background-color: #f5f5f5;
}

.edit-hint {
  font-size: 24rpx;
  color: #999;
  margin-left: 10rpx;
  font-weight: normal;
}

.memo-title-edit {
  margin-bottom: 20rpx;
}

.memo-title-edit input {
  font-size: 40rpx;
  font-weight: bold;
  color: #333;
  border-bottom: 2rpx solid #eee;
  padding: 10rpx 0;
  width: 100%;
}

.memo-time {
  font-size: 28rpx;
  color: #999;
  margin-bottom: 40rpx;
}

.memo-content {
  font-size: 32rpx;
  color: #333;
  line-height: 1.6;
  white-space: pre-wrap;
  position: relative;
  padding: 10rpx;
  border-radius: 8rpx;
}

.memo-content:active {
  background-color: #f5f5f5;
}

.edit-hint-content {
  position: absolute;
  bottom: 10rpx;
  right: 10rpx;
  font-size: 24rpx;
  color: #999;
  background-color: rgba(255, 255, 255, 0.8);
  padding: 4rpx 10rpx;
  border-radius: 4rpx;
}

.memo-content-edit textarea {
  width: 100%;
  font-size: 32rpx;
  color: #333;
  line-height: 1.6;
  min-height: 300rpx;
  border: 2rpx solid #eee;
  border-radius: 12rpx;
  padding: 20rpx;
  box-sizing: border-box;
}

.edit-buttons {
  display: flex;
  justify-content: space-between;
  margin-top: 40rpx;
}

.cancel-btn, .save-btn {
  width: 45%;
  height: 80rpx;
  border-radius: 40rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28rpx;
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