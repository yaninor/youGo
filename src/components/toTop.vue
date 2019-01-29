<template>
  <div>
    <div class="bottom-line">
      <i class="iconfont icon-xiao"></i>
      <span>我是有底线的</span>
    </div>
    <div class="toTop" v-show="isShow" @click="toTop">
      <i class="iconfont icon-jiantoushang"></i>
      <p>顶部</p>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
        isShow:false
    };
  },
    onPageScroll(e) {
    e.scrollTop > 200 ? (this.isShow = true) : (this.isShow = false);
  },
  methods: {
    toTop() {
      wx.pageScrollTo({
        scrollTop: 0,
        duration: 300
      });
    }
  }
};
</script>
<style scoped lang="scss">
.bottom-line {
  display: flex;
  color: #999;
  justify-content: center;
  align-items: center;
  height: 100rpx;
  background-color: #f4f4f4;
  i {
    font-size: 24rpx;
  }
  span {
    font-size: 24rpx;
  }
}
.toTop {
  width: 90rpx;
  height: 90rpx;
  text-align: center;
  border-radius: 50%;
  color: #666;
  position: fixed;
  right: 10rpx;
  bottom: 120rpx;
  background-color: #fff;
  padding: 5rpx;
  z-index: 999;
  p {
    font-size: 24rpx;
  }
}
</style>
