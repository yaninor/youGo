<template>
  <div class="category-container">
    <searchBox></searchBox>
    <div class="scroll-box">
      <scroll-view class="left" scroll-y scroll-with-animation>
        <ul>
          <li
            v-for="(item, i) in categoryList"
            :key="item.cat_id"
            :class="{actived:index===i}"
            @click="index=i"
          >{{item.cat_name}}</li>
        </ul>
      </scroll-view>
      <scroll-view class="right" scroll-y scroll-with-animation>
        <img src="/static/titleImage.png" alt class="title-img">
        <block v-if="categoryList.length!=0">
          <div class="section" v-for="level1 in categoryList[index].children" :key="level1.cat_id">
            <div class="title">
              <span>/</span>
              {{level1.cat_name}}
              <span>/</span>
            </div>
            <div class="items">
              <div class="item" v-for="(level2,i) in level1.children" :key="level2.cat_id">
                <img :src="'https://autumnfish.cn/wx/'+level2.cat_icon">
                <p>{{level2.cat_name}}</p>
              </div>
            </div>
          </div>
        </block>
      </scroll-view>
    </div>
  </div>
</template>

<script>
import searchBox from "../../components/searchBox";
import hxios from "../../utils/index.js";
export default {
  data() {
    return {
      categoryList: [],
      index: 0
    };
  },
  async onLoad() {
    let res = await hxios.get({
      url: "api/public/v1/categories"
    });
    console.log(res);
    this.categoryList = res.data.message;
  },
  components: {
    searchBox
  }
};
</script>

<style lang="scss">
$uRed: #ff2d4a;
page {
  height: 100%;
}
.category-container {
  height: 100%;
  padding-top: 100rpx;
  box-sizing: border-box;
  .scroll-box {
    display: flex;
    height: 100%;
    scroll-view {
      height: 100%;
      ::-webkit-scrollbar {
        width: 0;
        height: 0;
        color: transparent;
      }
      &.left {
        width: 200rpx;
        height: 100%;
        // background-color: hotpink;
        ul {
          li {
            height: 100rpx;
            line-height: 100rpx;
            text-align: center;
            background-color: #f4f4f4;
            font-size: 30rpx;
            &.actived {
              background-color: #fff;
              color: $uRed;
              font-weight: 700;
              position: relative;
              &::before {
                content: "";
                position: absolute;
                width: 10rpx;
                height: 60rpx;
                left: 0;
                top: 20rpx;
                background-color: $uRed;
              }
            }
          }
        }
      }
      &.right {
        flex: 1;
        height: 100%;
        padding: 15rpx;
        box-sizing: border-box;
        .title-img {
          display: block;
          width: 100%;
          height: 200rpx;
        }
        .section {
          margin-top: 40rpx;
        }
        .title {
          text-align: center;
          font-size: 28rpx;
          span {
            margin: 0 40rpx;
            color: #ccc;
          }
        }
        .items {
          display: flex;
          flex-wrap: wrap;
          margin-top: 20rpx;
          .item {
            width: 33.33%;
            img {
              display: block;
              width: 100rpx;
              height: 70rpx;
              margin: 0 auto;
            }
            p {
              text-align: center;
              margin-bottom: 20rpx;
              font-size: 26rpx;
            }
          }
        }
      }
    }
  }
}
</style>
