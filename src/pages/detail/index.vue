<template>
  <div class="detail-container">
    <div class="swiper-container">
      <swiper indicator-dots autoplay circular indicator-active-color="#fff">
        <swiper-item v-for="item in goodsInfo.pics" :key="item.pics_id">
          <img :src="item.pics_mid_url" @click="preview(item.pics_big_url)">
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
          <span @click="getAddress">
            <!-- 广东省 广东市 海珠区 -->
            {{address}}
            <i class="iconfont icon-jiantouyou"></i>
          </span>
        </div>
      </div>
      <div class="details">
        <div class="top">
          <div class="intro" :class="{active:index===0}" @click="index=0">图文介绍</div>
          <div class="options" :class="{active:index===1}" @click="index=1">规格参数</div>
        </div>
        <div class="bottom">
          <div class="content" v-html="goodsInfo.goods_introduce" v-show="index===0"></div>
          <div class="content" v-show="index===1">
            <div class="parameter" v-for="(item, index) in goodsInfo.attrs" :key="item.attr_id">
              <div class="parameter-name">{{item.attr_name}}</div>
              <div class="parameter-value">{{item.attr_value}}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="bottom-tool">
      <div class="tool1">
        <i class="iconfont icon-kefu"></i>联系客服
      </div>
      <div class="tool2" @click="toCart">
        <i class="iconfont icon-gouwuche"></i>购物车
      </div>
      <div class="tool3" @click="add2cart(goodsInfo.goods_id)">加入购物车</div>
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
      goodsInfo: {},
      address: "",
      index: 0
    };
  },
  components: {
    toTop
  },
  async onLoad(options) {
    // console.log(options.goods_id);
    //获取商品详情
    let res = await hxios.get({
      url: "api/public/v1/goods/detail",
      data: {
        goods_id: options.goods_id
      }
    });
    this.goodsInfo = res.data.message;
    console.log(res);
    //读取缓存地址
    wx.getStorage({
      key: "address",
      success: res => {
        this.address =
          res.data.provinceName +
          "　" +
          res.data.cityName +
          "　" +
          res.data.countyName;
      }
    });
  },
  methods: {
    //大图预览
    preview(pics_big_url) {
      let picList = this.goodsInfo.pics.map(v => v.pics_big_url);
      // console.log(this.picList);
      wx.previewImage({
        current: pics_big_url, // 当前显示图片的http链接
        urls: picList // 需要预览的图片http链接列表
      });
    },
    //获取地址
    getAddress() {
      wx.chooseAddress({
        success: res => {
          // console.log(res.provinceName);
          // console.log(res.cityName);
          // console.log(res.countyName);
          this.address =
            res.provinceName + "　" + res.cityName + "　" + res.countyName;
          //缓存地址
          wx.setStorage({
            key: "address",
            data: res
          });
        }
      });
    },
    //去购物车
    toCart() {
      wx.switchTab({ url: "/pages/cart/main" });
    },
    //加入购物车
    add2cart(id){
      wx.getStorage({
        key: 'cart',
        success: (res) => {
          // console.log(res.data)
          //判断是否有商品数据 有就累加 没有就设置为1
          if(res.data[id]){
            res.data[id]++;
          }else{
            res.data[id] = 1;
          }
          wx.setStorage({
            key: 'cart',
            data: res.data
          });
        },
        fail: () => {
          //没有商品数据就添加
          let cartData = {};
          cartData[id] = 1;
          wx.setStorage({
            key: 'cart',
            data: cartData
          });
         },
        complete: () => { 
          wx.showToast({
            title: '添加成功', //提示的内容,
            icon: 'success', //图标,
            duration: 2000, //延迟时间,
            mask: true, //显示透明蒙层，防止触摸穿透,
            success: res => {}
          });
        }
      })
    }
  }
};
</script>

<style scoped lang="scss">
$uRed: #ff2d4a;
.detail-container {
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
    font-weight: 600;
  }
  .title {
    width: 100%;
    display: flex;
    height: 80rpx;
    font-weight: 500;
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
    padding: 0 15rpx;
    margin-top: 20rpx;
    width: 100%;
    .top {
      height: 100rpx;
      display: flex;
      text-align: center;
      line-height: 100rpx;
      .intro {
        flex: 1;
      }
      .options {
        flex: 1;
      }
      .active {
        color: $uRed;
        box-sizing: border-box;
        border-bottom: 10rpx solid $uRed;
      }
    }
    .bottom {
      padding-bottom: 40rpx;
      .content {
        margin-top: 12rpx;
        .parameter {
          display: flex;
          font-size: 28rpx;
          height: 85rpx;
          line-height: 85rpx;
          position: relative;
          margin-top: -2rpx;
          .parameter-name {
            flex: 1;
            text-align: center;
            border: 2rpx solid #ccc;
          }
          .parameter-value {
            flex: 1;
            border: 2rpx solid #ccc;
            position: relative;
            margin-left: -2rpx;
            padding-left: 40rpx;
          }
        }
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
    flex: 2;
    font-size: 24rpx;
    padding-top: 10rpx;
    i {
      font-size: 40rpx;
    }
  }
  .tool2 {
    flex: 2;
    padding-top: 10rpx;
    font-size: 24rpx;
    i {
      font-size: 40rpx;
    }
  }
  .tool3 {
    flex: 3;
    line-height: 100rpx;
    background-color: #ffb400;
    font-size: 28rpx;
    color: #fff;
  }
  .tool4 {
    line-height: 100rpx;
    flex: 3;
    font-size: 28rpx;
    background-color: $uRed;
    color: #fff;
  }
}
</style>
