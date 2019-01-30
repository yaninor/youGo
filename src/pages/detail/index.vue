<template>
  <div class="detail-container">
    <div class="swiper-container">
      <swiper indicator-dots autoplay circular indicator-active-color="#fff">
        <swiper-item v-for="item in goodsInfo.pics" :key="item.pics_id">
          <img :src="item.pics_mid">
        </swiper-item>
      </swiper>
    </div>
    <div class="goods-details">
      <div class="price">¥{{goodsInfo.goods_price}}</div>
      <div class="title">
        <div class="name">{{goodsInfo.goods_name}}</div>
        <div class="collect">
          <i class="iconfont icon-shoucang"></i>
          <span>收藏</span>
        </div>
      </div>
      <div class="express">
        <span>快递：</span>
        <span>免运费</span>
      </div>
      <div class="item1">
        <div class="promotion">
          促销
          <span>满300减30</span>
        </div>
        <div class="selectd">
          已选
          <span>黑色/S/1件</span>
        </div>
      </div>
      <div class="item2">
        <div class="address">
          送至
          <span>
            广东省 广东市 海珠区
            <i class="iconfont icon-jiantouyou"></i>
          </span>
        </div>
      </div>
      <div class="details">
        <div class="top">
          <div class="intro actived">图文介绍</div>
          <div class="options">规格参数</div>
        </div>
        <div class="bottom">
          <span>{{goodsInfo.goods_name}}</span>
          <img :src="item.pics_mid" alt v-for="(item, index) in goodsInfo.pics" :key="item.pics_id">
        </div>
      </div>
    </div>
    <div class="bottom-tool">
      <div class="tool1">
        <i class="iconfont icon-kefu"></i>联系客服
      </div>
      <div class="tool2">
        <i class="iconfont icon-gouwuche"></i>购物车
      </div>
      <div class="tool3">加入购物车</div>
      <div class="tool4">立即购买</div>
    </div>
    <toTop></toTop>
  </div>
</template>

<script>
import hxios from "../../utils/index.js";
import toTop from "../../components/toTop";
export default {
  data() {
    return {
      goodsInfo: {}
    };
  },
  components: {
    toTop
  },
  async onLoad(options) {
    // console.log(options.goods_id);
    let res = await hxios.get({
      url: "api/public/v1/goods/detail",
      data: {
        goods_id: options.goods_id
      }
    });
    // console.log(res);
    this.goodsInfo = res.data.message;
  }
};
</script>

<style scoped lang="scss">
$uRed: #ff2d4a;
page {
  background-color: #f4f4f4;
  padding-bottom: 100rpx;
  box-sizing: border-box;
  width: 100%;
}
.swiper-container {
  box-sizing: border-box;
  width: 100%;
  height: auto;
  swiper {
    width: 100%;
    height: 720rpx;
    swiper-item {
      height: 100%;
      img {
        width: 100%;
        height: 100%;
        display: block;
      }
    }
  }
}
.goods-details {
  width: 100%;
  box-sizing: border-box;
  color: #404040;
  > div {
    background-color: #fff;
    padding-left: 20rpx;
    box-sizing: border-box;
  }
  .price {
    width: 100%;
    height: 120rpx;
    font-size: 36rpx;
    padding: 40rpx 0 40rpx 20rpx;
    box-sizing: border-box;
    color: $uRed;
  }
  .title {
    width: 100%;
    display: flex;
    height: 80rpx;
    .name {
      height: 100%;
      flex: 1;
      font-size: 28rpx;
      padding-right: 80rpx;
      box-sizing: border-box;
      border-right: 1rpx solid #ccc;
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 2;
    }
    .collect {
      width: 140rpx;
      text-align: center;
      height: 100%;
      color: #999;
      i {
        font-size: 28rpx;
      }
      span {
        font-size: 28rpx;
      }
    }
  }
  .express {
    font-size: 28rpx;
    color: #999;
    padding-top: 40rpx;
    width: 100%;
    height: 100rpx;
  }
  .item1 {
    margin-top: 20rpx;
    height: 200rpx;
    width: 100%;
    font-size: 30rpx;
    span {
      line-height: 100rpx;
      margin-left: 40rpx;
    }
    .promotion {
      span {
        color: $uRed;
      }
    }
    .selectd {
      span {
        color: #999;
      }
    }
  }
  .item2 {
    width: 100%;
    height: 100rpx;
    line-height: 100rpx;
    margin-top: 20rpx;
    .address {
      font-size: 30rpx;
      span {
        margin-left: 40rpx;
        color: #999;
        i {
          display: inline;
        }
      }
    }
  }
  .details {
    box-sizing: border-box;
    padding: 0 10rpx;
    margin-top: 20rpx;
    width: 100%;
    .top {
      height: 100rpx;
      display: flex;
      font-size: 28rpx;
      text-align: center;
      line-height: 100rpx;
      .intro {
        flex: 1;
      }
      .options {
        flex: 1;
      }
      .actived {
        color: $uRed;
        box-sizing: border-box;
        border-bottom: 10rpx solid $uRed;
      }
    }
    .bottom {
      img {
        width: 100%;
        height: 720rpx;
      }
    }
  }
}
.bottom-tool {
  display: flex;
  width: 100%;
  height: 100rpx;
  position: fixed;
  bottom: 0;
  left: 0;
  background-color: #fff;
  text-align: center;
  .tool1 {
    flex: 1;
    font-size: 24rpx;
    padding-top: 10rpx;
    i {
      font-size: 40rpx;
    }
  }
  .tool2 {
    flex: 1;
    padding-top: 10rpx;
    font-size: 24rpx;
    i {
      font-size: 40rpx;
    }
  }
  .tool3 {
    flex: 1;
    line-height: 100rpx;
    background-color: #ffb400;
    font-size: 28rpx;
    color: #fff;
  }
  .tool4 {
    line-height: 100rpx;
    flex: 1;
    font-size: 28rpx;
    background-color: $uRed;
    color: #fff;
  }
}
</style>
