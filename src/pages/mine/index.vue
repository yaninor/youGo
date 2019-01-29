<template>
  <div class="mine">
    <div class="header">
      <div class="top">
        <i class="iconfont icon-shezhi"></i>
        <button open-type="getUserInfo" @getuserinfo="getUserInfo">
          <img :src="picUrl" alt>
        </button>
        <i class="iconfont icon-xiaoxi"></i>
      </div>
      <p>{{message}}</p>
    </div>
    <div class="body">
      <div class="history">
        <div class="history-item">
          <p>0</p>
          <p>收藏的店铺</p>
        </div>
        <div class="history-item">
          <p>0</p>
          <p>收藏的商品</p>
        </div>
        <div class="history-item">
          <p>0</p>
          <p>关注的商品</p>
        </div>
        <div class="history-item">
          <p>0</p>
          <p>我的足迹</p>
        </div>
      </div>
      <div class="order">
        <div class="title">我的订单</div>
        <div class="order-item">
          <div class="item">
            <i class="iconfont icon-daifukuan"></i>
            <p>待付款</p>
          </div>
          <div class="item">
            <i class="iconfont icon-daishouhuo"></i>
            <p>待收货</p>
          </div>
          <div class="item">
            <i class="iconfont icon-tuikuan"></i>
            <p>退款/退货</p>
          </div>
          <div class="item">
            <i class="iconfont icon-dingdan"></i>
            <p>全部订单</p>
          </div>
        </div>
      </div>
      <div class="address" @click="getAddress">
        <p>收货地址管理</p>
        <i class="iconfont icon-jiantouyou"></i>
      </div>
      <div class="bottom">
        <div class="bottom-item" @click="call">
          <p>联系客服</p>
          <span>400-618-4000</span>
        </div>
        <div class="bottom-item">
          <p>意见反馈</p>
        </div>
        <div class="bottom-item">
          <p>关于我们</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "登录/注册",
      picUrl: "/static/icon.png"
    };
  },
  methods: {
    getUserInfo(event) {
      // console.log(event);
      this.picUrl = event.mp.detail.userInfo.avatarUrl;
      this.message = event.mp.detail.userInfo.nickName;
    },
    getAddress() {
      wx.chooseAddress({
        success(res) {
          console.log(res.userName);
          console.log(res.postalCode);
          console.log(res.provinceName);
          console.log(res.cityName);
          console.log(res.countyName);
          console.log(res.detailInfo);
          console.log(res.nationalCode);
          console.log(res.telNumber);
        }
      });
    },
    call() {
      wx.makePhoneCall({
        phoneNumber: "13670225518" // 仅为示例，并非真实的电话号码
      });
    }
  }
};
</script>

<style lang="scss">
$uRed: #ff2d4a;
page {
  background-color: #f4f4f4;
}
.header {
  width: 100%;
  background-color: $uRed;
  height: 410rpx;
  .top {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 100rpx 220rpx 0;
    i {
      font-size: 30rpx;
      color: #fff;
    }
    button {
      border-radius: 50%;
      padding: 0;
      border: 5rpx solid #fff;
      img {
        display: block;
        height: 128rpx;
        width: 128rpx;
      }
    }
  }
  p {
    text-align: center;
    color: #fff;
    font-size: 28rpx;
    margin-top: 20rpx;
  }
}
.body {
  margin-top: -20rpx;
  margin-left: 15rpx;
  width: 720rpx;
  > div {
    background-color: #fff;
  }
  .history {
    display: flex;
    padding: 30rpx;
    color: #666;
    height: 120rpx;
    background-color: #fff;
    box-sizing: border-box;
    .history-item {
      flex: 1;
      font-size: 24rpx;
      text-align: center;
    }
  }
  .order {
    margin-top: 20rpx;
    width: 100%;
    .title {
      height: 90rpx;
      width: 100%;
      font-size: 32rpx;
      padding: 0 30rpx;
      line-height: 90rpx;
      color: #313131;
      border: 1rpx solid #ebeaeb;
      box-sizing: border-box;
    }
    .order-item {
      display: flex;
      .item {
        flex: 1;
        height: 165rpx;
        width: 100%;
        text-align: center;
        color: #313131;
        padding: 30rpx;
        box-sizing: border-box;
        i {
          font-size: 50rpx;
          color: $uRed;
        }
        p {
          font-size: 24rpx;
          margin-top: 20rpx;
        }
      }
    }
  }
  .address {
    margin-top: 20rpx;
    width: 100%;
    height: 86rpx;
    font-size: 32rpx;
    position: relative;
    padding: 0 30rpx;
    box-sizing: border-box;
    p {
      color: #313131;
      line-height: 86rpx;
    }
    i {
      position: absolute;
      right: 10rpx;
      top: 50%;
      transform: translateY(-50%);
      color: #ccc;
    }
  }
  .bottom {
    margin-top: 20rpx;
    padding: 0 30rpx;
    .bottom-item {
      height: 85rpx;
      width: 100%;
      position: relative;
      box-sizing: border-box;
      font-size: 32rpx;
      line-height: 85rpx;
      border-bottom: 1px solid #ebeaeb;
      &:last-child {
        border-bottom: none;
      }
      p {
        color: #313131;
        line-height: 70rpx;
      }
      span {
        color: #999;
        position: absolute;
        right: 20rpx;
        top: 50%;
        transform: translateY(-50%);
      }
    }
  }
}
</style>
