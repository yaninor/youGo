<template>
  <div class="index-container">
    <serarchBox></serarchBox>
    <div class="swiper-container">
      <swiper indicator-dots autoplay circular indicator-active-color="#fff">
        <swiper-item v-for="item in swiperList" :key="item.image_src">
          <img :src="item.image_src">
        </swiper-item>
      </swiper>
    </div>
    <div class="category-container">
      <div class="category-item" v-for="item in categoryList" :key="item.image_src">
        <img :src="item.image_src" alt>
        <p>{{item.name}}</p>
      </div>
    </div>
    <div class="floor-container">
      <div class="floor-item" v-for="item in floorList" :key="item.floor_title.image_src">
        <div class="title">
          <p>{{item.floor_title.name}}</p>
          <img :src="item.floor_title.image_src" alt>
        </div>
        <div class="right">
          <a href="#" v-for="(i,idx) in item.product_list" :key="i.image_src">
            <img :src="i.image_src" alt>
          </a>
        </div>
      </div>
    </div>
    <div class="bottom-line">
      <i class="iconfont icon-xiao"></i>
      <span>我是有底线的</span>
    </div>
    <toTop></toTop>
  </div>
</template>

<script>
import hxios from "../../utils/index.js";
import serarchBox from "../../components/searchBox";
import toTop from '../../components/toTop'
export default {
  data() {
    return {
      swiperList: [],
      categoryList: [],
      floorList: []
    };
  },
  components: {
    serarchBox,
    toTop
  },
  async onLoad() {
    let p1 = hxios.get({
      url: "api/public/v1/home/swiperdata"
    });
    let p2 = hxios.get({
      url: "api/public/v1/home/catitems"
    });
    let p3 = hxios.get({
      url: "api/public/v1/home/floordata"
    });
    let totalRes = await Promise.all([p1, p2, p3]);
    // console.log(totalRes);
    this.swiperList = totalRes[0].data.message;
    this.categoryList = totalRes[1].data.message;
    this.floorList = totalRes[2].data.message;
  }
};
</script>

<style scoped lang="scss">
$uRed: #ff2d4a;
.index-container {
  padding-top: 100rpx;
  .search-box {
    padding: 20rpx 16rpx;
    background-color: $uRed;
    box-sizing: border-box;
    position: fixed;
    width: 100%;
    top: 0;
    left: 0;
    z-index: 999;
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
  .swiper-container {
    swiper {
      width: 100%;
      height: 340rpx;
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
  .category-container {
    display: flex;
    padding-top: 24rpx;
    padding-bottom: 29rpx;
    background-color: #fff;
    .category-item {
      flex: 1;
      img {
        width: 128rpx;
        height: 128rpx;
        display: block;
        margin: 0 auto;
      }
      p {
        text-align: center;
        margin-top: 10rpx;
        font-size: 24rpx;
      }
    }
  }
  .floor-container {
    .floor-item {
      .title {
        position: relative;
        img {
          width: 100%;
          height: 90rpx;
        }
        p {
          position: absolute;
          left: 20rpx;
          top: 50%;
          transform: translateY(-50%);
          font-size: 34rpx;
          color: #ff7b94;
        }
      }
      .right {
        overflow: hidden;
        padding: 20rpx 0 0 16rpx;
        a {
          display: block;
          width: 33.33%;
          height: 190rpx;
          position: relative;
          float: right;
          box-sizing: border-box;
          border-radius: 10rpx;
          &:nth-child(1) {
            float: left;
            height: 390rpx;
            width: 230rpx;
            img {
              width: 100%;
              height: 390rpx;
              display: block;
            }
          }
          &:nth-child(2),
          &:nth-child(3) {
            margin-bottom: 10rpx;
          }
          img {
            width: 240rpx;
            height: 190rpx;
            border-radius: 10rpx;
          }
        }
      }
    }
  }
}
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
</style>
