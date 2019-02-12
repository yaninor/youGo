<template>
  <div class="search-container">
    <div class="search">
      <icon type="search" size="15"></icon>
      <input
        type="text"
        placeholder="请输入你想要的商品"
        v-model.trim="inputValue"
        @confirm="getInputValue"
        @focus="isShow=true"
      >
      <button @click="back">取消</button>
    </div>
    <div class="history" v-show="isShow">
      <div class="title">
        <h2>历史搜索</h2>
        <i class="iconfont icon-shanchu" @click="clear"></i>
      </div>
      <div class="history-item">
        <div class="item">
          <p v-for="(item, index) in searchList" @click="query(item)">
            {{item}}
            <i class="iconfont icon-shanchu" @click="delOne(index)"></i>
          </p>
        </div>
      </div>
    </div>
    <div class="goods" v-show="!isShow">
      <div class="sort">
        <p :class="{active:orderIndex===0}" @click="orderIndex=0">综合</p>
        <p :class="{active:orderIndex===1}" @click="orderIndex=1">销量</p>
        <p :class="{active:orderIndex===2}" @click="orderIndex=2;isUp=!isUp">
          价格
          <span :class="{active:orderIndex===2&&isUp}">▲</span>
          <span :class="{active:orderIndex===2&&!isUp}">▼</span>
        </p>
      </div>
      <div class="goods-item">
        <div
          class="item"
          v-for="(item, index) in sortList"
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
      searchList: [],
      resultList: [],
      isShow: false,
      orderIndex: 0,
      isUp: true
    };
  },
  components: {
    toTop
  },
  methods: {
    async getInputValue() {
      if (this.inputValue === "") {
        return;
      }
      //长度限制
      if (this.searchList.length >= 10) {
        this.searchList.shift();
      }
      //去重
      if (this.searchList.indexOf(this.inputValue) != -1) {
        this.searchList.splice(this.searchList.indexOf(this.inputValue), 1);
      }
      this.isShow = false;
      //添加记录
      this.searchList.push(this.inputValue);
      let res = await hxios.get({
        url: "api/public/v1/goods/search?query=",
        data: {
          query: this.inputValue
        }
      });
      // console.log(res);
      this.resultList = res.data.message.goods;
      this.inputValue = "";
    },
    //查询商品
    async query(value) {
      this.inputValue = value;
      this.getInputValue();
    },
    //删除一条记录
    delOne(index) {
      this.searchList.splice(index, 1);
    },
    // 清空所有历史
    clear() {
      wx.showModal({
        title: "提示",
        content: "你确定要清空历史?O(∩_∩)O",
        // 默认是 success(){} this已经改变 使用箭头函数 固定this
        success: res => {
          if (res.confirm) {
            // console.log("用户点击确定");
            this.searchList = [];
          } else if (res.cancel) {
            // console.log("用户点击取消");
          }
        }
      });
    },
    //返回上一页
    back() {
      wx.navigateBack({
        delta: 1 //返回的页面数，如果 delta 大于现有页面数，则返回到首页,
      });
    },
    // 获取商品信息
    getGoodsInfo(data) {
      // console.log(data);
      wx.navigateTo({ url: "/pages/detail/main?goods_id=" + data.goods_id });
    }
  },
  // created 只要保证data有了即可
  created() {
    let res = wx.getStorageSync("history");
    // console.log(res);
    if (res) {
      this.searchList = res;
    }
    this.isShow = true;
  },
  // 侦听器
  watch: {
    searchList() {
      // console.log("我变了");
      wx.setStorage({
        key: "history",
        data: this.searchList
      });
    }
  },
  //计算属性
  computed: {
    //列表排序
    sortList() {
      if(this.orderIndex===0){
        return this.resultList;
      }else if(this.orderIndex===1){
       return this.resultList.slice(0).sort((a,b)=>{
         return a.goods_id - b.goods_id;
       })
      }else if(this.orderIndex===2&&this.isUp){
        return this.resultList.slice(0).sort((a,b)=>{
          return a.goods_price - b.goods_price;
        })
      }else if(this.orderIndex===2&&!this.isUp){
        return this.resultList.slice(0).sort((a,b)=>{
          return b.goods_price - a.goods_price
        })
      }
      // switch (this.orderIndex) {
      //   case 0:
      //     return this.resultList;
      //     break;
      //   case 1:
      //     let newArr = JSON.parse(JSON.stringify(this.resultList));
      //     return newArr.sort((a, b) => {
      //       return a.goods_id - b.goods_id;
      //     });
      //     break;
      //   case 2:
      //     return newArr.sort((a, b) => {
      //       newArr = JSON.parse(JSON.stringify(this.resultList));
      //       if (this.isUp) {
      //         return a.goods_price - b.goods_price;
      //       } else {
      //         return b.goods_price - a.goods_price;
      //       }
      //     })
      //     break;
      // }
    }
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
    background-color: $uRed;
    color: #fff;
  }
}
.history {
  width: 100%;
  padding: 0 30rpx 0 16rpx;
  box-sizing: border-box;
  position: absolute;
  top: 120rpx;
  left: 0;
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
      &.active {
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
          color: #ccc;

          &:nth-child(1) {
            top: -5rpx;
          }
          &:nth-child(2) {
            top: 10rpx;
          }
        }
        .active {
          color: $uRed;
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
