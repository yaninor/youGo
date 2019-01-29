<template>
  <div class="search-container">
    <div class="search">
      <icon type="search" size="15"></icon>
      <input type="text" placeholder="请输入你想要的商品" v-model.trim="inputValue" @change="getInputValue">
      <button @click="back">取消</button>
    </div>
    <div class="history" v-if="resultList.length===0">
      <div class="title">
        <h2>历史搜索</h2>
        <i class="iconfont icon-shanchu" @click="clear"></i>
      </div>
      <div class="history-item">
        <div class="item">
          <p v-for="(item, index) in historyList" @click="query(item)">
            {{item}}
            <i class="iconfont icon-shanchu" @click="delOne(index)"></i>
          </p>
        </div>
      </div>
    </div>
    <div class="goods" v-else-if="resultList.length!=0">
      <div class="sort">
        <p class="actived">综合</p>
        <p>销量</p>
        <p>
          价格
          <span>▲</span>
          <span>▼</span>
        </p>
      </div>
      <div class="goods-item">
        <div
          class="item"
          v-for="(item, index) in resultList"
          :key="item.goods_id"
          @click="getGoodsInfo(item)"
        >
          <div class="left">
            <img :src="item.goods_small_logo" alt>
          </div>
          <div class="right">
            <p>{{item.goods_name}}</p>
            <div class="price">
              ¥
              <span>{{item.goods_price}}</span>.00
            </div>
          </div>
        </div>
      </div>
      <toTop></toTop>
    </div>
  </div>
</template>

<script>
import hxios from "../../utils/index.js";
import toTop from "../../components/toTop";
export default {
  data() {
    return {
      inputValue: "",
      historyList: [],
      resultList: []
    };
  },
  components: {
    toTop
  },
  methods: {
    async getInputValue() {
      console.log(this.inputValue);
      if (this.inputValue === "") {
        return;
      }
      //长度限制
      if (this.historyList.length >= 10) {
        this.historyList.shift();
      }
      //去重
      if (this.historyList.indexOf(this.inputValue) != -1) {
        this.historyList.splice(this.historyList.indexOf(this.inputValue), 1);
      }
      //添加记录
      this.historyList.push(this.inputValue);
      let res = await hxios.get({
        url: "api/public/v1/goods/search?query=",
        data: {
          query: this.inputValue
        }
      });
      console.log(res);
      this.resultList = res.data.message.goods;
      this.inputValue = "";
    },
    async query(value) {
      this.inputValue = value;
      this.getInputValue();
    },
    delOne(index) {
      this.historyList.splice(index, 1);
    },
    clear() {
      this.historyList = [];
    },
    back() {
      wx.navigateBack({
        delta: 1 //返回的页面数，如果 delta 大于现有页面数，则返回到首页,
      });
    },
    getGoodsInfo(data) {
      console.log(data);
      wx.navigateTo({ url: "/pages/detail/main?goods_id=" + data.goods_id });
    }
  },
  watch: {
    historyList() {
      wx.setStorage({
        key: "history",
        data: this.historyList
      });
    }
  },
  created() {
    this.historyList = wx.getStorageSync("history");
  }
};
</script>

<style lang="scss">
$uRed: #ff2d4a;
.search {
  display: flex;
  height: 120rpx;
  width: 100%;
  background-color: #eeeeee;
  padding: 30rpx 15rpx;
  box-sizing: border-box;
  position: relative;
  input {
    background-color: #fff;
    border-radius: 5rpx;
    height: 60rpx;
    font-size: 28rpx;
    width: 538rpx;
    padding-left: 70rpx;
  }
  icon {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    left: 37rpx;
  }
  button {
    padding: 0;
    width: 158rpx;
    font-size: 30rpx;
    margin-left: 20rpx;
    height: 58rpx;
    line-height: 58rpx;
    background-color: #eee;
    color: #aaa;
    // padding: 0 20rpx;
  }
}
.history {
  width: 100%;
  padding: 0 30rpx 0 16rpx;
  box-sizing: border-box;
  position: absolute;
  top:120rpx;
  left:0;
  .title {
    height: 88rpx;
    position: relative;
    h2 {
      line-height: 88rpx;
      font-size: 30rpx;
      color: #333;
    }
    i {
      font-size: 30rpx;
      position: absolute;
      right: 0;
      top: 50%;
      transform: translateY(-50%);
      color: #ccc;
    }
  }
  .history-item {
    width: 100%;
    .item {
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      p {
        padding: 20rpx 26rpx;
        background-color: #dddddd;
        font-size: 24rpx;
        margin-right: 30rpx;
        margin-bottom: 18rpx;
        position: relative;
        i {
          position: absolute;
          right: 0;
          top: 0;
          transform: translate(50%, -50%);
          color: $uRed;
        }
      }
    }
  }
}
.goods {
  width: 100%;
  .sort {
    display: flex;
    height: 100rpx;
    width: 100%;
    border-bottom: 1rpx solid #ccc;
    background-color: #fff;
    p {
      flex: 1;
      font-size: 30rpx;
      color: #353535;
      line-height: 100rpx;
      text-align: center;
      width: 100%;
      height: 100%;
      &.actived {
        color: $uRed;
      }
      &:last-child {
        position: relative;
        span {
          height: 100%;
          position: absolute;
          font-size: 20rpx;
          display: block;
          transform: scaleY(0.5);
          top: 0;
          right: 75rpx;
          &:nth-child(1) {
            top: -5rpx;
            color: #ccc;
          }
          &:nth-child(2) {
            top: 10rpx;
          }
        }
      }
    }
  }
  .goods-item {
    width: 100%;
    padding-left: 20rpx;
    box-sizing: border-box;
    .item {
      display: flex;
      height: 260rpx;
      border-bottom: 1rpx solid #ccc;
      padding: 30rpx 30rpx 30rpx 0;
      box-sizing: border-box;
      .left {
        width: 200rpx;
        height: 100%;
        margin-right: 20rpx;
        img {
          width: 100%;
          height: 200rpx;
          display: block;
        }
      }
      .right {
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        p {
          font-size: 30rpx;
          overflow: hidden;
          text-overflow: ellipsis;
          display: -webkit-box;
          -webkit-box-orient: vertical;
          -webkit-line-clamp: 2;
        }
        .price {
          font-size: 20rpx;
          color: $uRed;
          span {
            font-size: 34rpx;
          }
        }
      }
    }
  }
}
.search-container {
  padding-top: 220rpx;
  position: relative;
  .search {
    position: fixed;
    top: 0;
    left: 0;
  }
  .sort {
    position: fixed;
    top: 120rpx;
    left: 0;
  }
}
</style>
