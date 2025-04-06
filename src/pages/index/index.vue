<template>
  <view class="memo-page">
    <view class="header">
      <view class="title-container">
        <view class="back-container">
          <text class="back-icon">&lt;</text>
        </view>
        <text class="title">我的备忘录</text>
      </view>
    </view>
    
    <view class="memo-container">
      <view class="memo-item" v-for="(item, index) in memoList" :key="index" @tap="viewMemoDetail(item)">
        <view class="memo-title">{{item.title}}</view>
        <view class="memo-content">{{item.content}}</view>
      </view>
    </view>
    
    <view class="add-button" @tap="createNewMemo">
      <text class="add-icon">+</text>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue';

// 模拟备忘录数据
const memoList = ref([
  {
    id: 1,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容...'
  },
  {
    id: 2,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容...'
  },
  {
    id: 3,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容...'
  },
  {
    id: 4,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容...'
  },
  {
    id: 5,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容...'
  },
  {
    id: 6,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容...'
  },
  {
    id: 7,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容...'
  },
  {
    id: 8,
    title: '标题标题标题',
    content: '内容内容内容内容内容内容内容内容内容内容内容内容...'
  }
]);

// 查看备忘录详情
const viewMemoDetail = (memo) => {
  uni.navigateTo({
    url: '/pages/memo-detail/index?id=' + memo.id
  });
};

// 创建新备忘录
const createNewMemo = () => {
  uni.navigateTo({
    url: '/pages/memo-edit/index'
  });
};
</script>

<style lang="scss">
.memo-page {
  width: 100%;
  min-height: 100vh;
  background: linear-gradient(135deg, #a6c0fe 0%, #c2a8fd 100%);
  position: relative;
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

.memo-container {
  display: flex;
  flex-wrap: wrap;
  padding: 30rpx;
  background: linear-gradient(135deg, #a6c0fe 0%, #c2a8fd 100%);
}

.memo-item {
  width: calc(50% - 20rpx);
  margin: 10rpx;
  height: 280rpx;
  background-color: #fff;
  border-radius: 20rpx;
  padding: 24rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.05);
  box-sizing: border-box;
  overflow: hidden;
}

.memo-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #333;
  margin-bottom: 16rpx;
}

.memo-content {
  font-size: 28rpx;
  color: #666;
  display: -webkit-box;
  -webkit-line-clamp: 6;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}

.add-button {
  position: fixed;
  right: 40rpx;
  bottom: 40rpx;
  width: 100rpx;
  height: 100rpx;
  background: linear-gradient(90deg, #5c7cfa, #845ef7);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 6rpx 20rpx rgba(92, 124, 250, 0.4);
  z-index: 99;
}

.add-icon {
  font-size: 60rpx;
  font-weight: 300;
  color: #fff;
}
</style>
