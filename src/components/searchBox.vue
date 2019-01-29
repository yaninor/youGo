<template>
  <div class="search-box">
    <input type="text" placeholder="搜索" @click="search" disabled>
    <icon type="search" size="12"></icon>
  </div>
</template>

<script>
export default {
  methods: {
    search() {
      wx.navigateTo({
        url: "/pages/search/main"
      });
    }
  }
};
</script>

<style lang="scss">
$uRed: #ff2d4a;
.search-box {
  padding: 20rpx 16rpx;
  background-color: $uRed;
  box-sizing: border-box;
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 999;
  height: 100rpx;
  input {
    width: 100%;
    height: 60rpx;
    background-color: #fff;
    padding-left: 380rpx;
    box-sizing: border-box;
    font-size: 24rpx;
    border-radius: 5rpx;
  }
  icon {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>
