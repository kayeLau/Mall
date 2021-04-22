<template>
  <view>
    <van-row>
      <van-col span="6">
        <van-sidebar :active-key="activeKey" class="sidebar">
          <van-sidebar-item
            v-for="(item, index) in category"
            :title="item.name"
            :key="index"
            @click="sideBarHeadle(index)"
          />
        </van-sidebar>
      </van-col>

      <my-popup
        :show="show"
        v-on:close="onClose"
        v-on:buyValueChange="buyValueChange"
        position="bottom"
        closeable
        round
        custom-style="height: 70%"
        close-icon="close"
        close-icon-position="top-right"
        desc="2"
        tags="标签"
        :price="[getprice()]"
      >
        <view slot="standard" class="standard-slot">
          <van-button
            v-for="(item, index) in popupData.standard"
            :type="[popupStandard === index ? 'primary' : 'info']"
            size="small"
            :key="index"
            @click="standardChange(index)"
            >{{ item }}</van-button
          >
        </view>

        <view slot="button" class="button-slot">
          <van-button type="primary" custom-class="button-group button-cart"
            >加入購物車</van-button
          >
          <van-button type="default" custom-class="button-group button-buy"
            >立即購買</van-button
          >
        </view>
      </my-popup>

      <van-col span="18">
        <scroll-view :scroll-top="scrollTop" scroll-y="true" id="scroll">
          <view
            class="goodsList"
            v-for="(list, index) in goodsList"
            :key="index"
          >
            <view>
              <h5>{{ list.name }}</h5>
            </view>
            <van-card
              v-for="item in list.data"
              :key="item.id"
              :price="item.specialPrice[0]"
              :title="item.title"
            >
              <view slot="footer">
                <van-button
                  type="default"
                  icon="cart-o"
                  color="#ff7d9b"
                  round
                  plain
                  @click="showPopup(index, item.id)"
                />
              </view>
            </van-card>
          </view>
        </scroll-view>
      </van-col>
    </van-row>
  </view>
</template>
<script>
import goodsData from "@/static/data/goodsData.json";
export default {
  data() {
    return {
      show: false, //popup是否顯示
      activeKey: 0, 
      scrollTop: 0, //距離頂端距離
      category: [], //分類列表
      goodsList: [], //商品列表
      goodListHeight: [], //商品列表高度
      buyValue: 0, //購買數量
      popupData: {}, //被選中,顯示在popup的商品
      popupStandard:0
    };
  },
  computed:{

  },
//---方法---
  methods: {
    init() {
      //初始化分類與商品例表
      this.category = goodsData.category;
      this.goodsList = goodsData.data;
    },
    setSideBarPoint() {
      //設置各分類的頁面錨點
      let reduce = 0;
      const query = uni.createSelectorQuery().in(this);
      query
        .selectAll(".goodsList")
        .boundingClientRect(data => {
          data.forEach(item => {
            this.goodListHeight.push(reduce);
            reduce += item.height;
          });
        })
        .exec();
    },
    sideBarHeadle(index) {
      //點擊跳轉指定錨點
      this.scrollTop = this.goodListHeight[index];
    },
    showPopup(type, id) {
      //打開popup層
      this.findgoodData(type, id);
      this.show = true;
    },
    findgoodData(type, id) {
      //查找商品資料
      this.popupData = this.goodsList[type].data.filter(
        item => item.id === id
      )[0];
      console.log(this.popupData);
    },
    onClose() {
      //關閉popup
      this.show = false;
      this.popupData = {};
      this.popupStandard = 0;
    },
    buyValueChange(val) {
      //記錄購買數量
      this.buyValue = val;
    },
    standardChange(val){
      //記錄購買規格
      this.popupStandard = val;
    },
    getprice(){
      if(this.popupData.specialPrice){
        return this.popupData.specialPrice[this.popupStandard]
      }
    }
  },

//---生命周期---
  onShow: function() {
    this.init();
  },
  onReady: function() {
    this.setSideBarPoint();
  }
};
</script>
<style>
.goodsList button {
  height: 60rpx;
  width: 60rpx;
  box-sizing: border-box;
  padding: 0;
  border: 2px solid #ff7d9b;
}
.goodsList h5 {
  color: #ccc;
  font-size: 20rpx;
  padding: 10rpx 30rpx;
}
#scroll {
  height: 100vh;
}
.button-group {
  width: 300rpx !important;
  height: 80rpx !important;
}
.button-cart {
  border-top-left-radius: 40rpx !important;
  border-bottom-left-radius: 40rpx !important;
}
.button-buy {
  border-top-right-radius: 40rpx !important;
  border-bottom-right-radius: 40rpx !important;
}
.button-slot {
  display: flex;
  justify-content: center;
}
.standard-slot {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}
</style>