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
              :price="item.specialPrice"
              :title="item.title"
            >
              <view slot="footer">
                <van-button
                  icon="cart-o"
                  color="#ff7d9b"
                  round
                  plain
                  @click="clickHeadle"
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
      activeKey: 0,
      scrollTop: 0,
      category: [],
      goodsList: [],
      goodListHeight: []
    };
  },
  watch: {
    activeKey: function(val) {
      console.log(val);
    }
  },
  methods: {
    init() {
      this.category = goodsData.category;
      this.goodsList = goodsData.data;
    },
    setSideBarPoint() {
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
    clickHeadle() {},
    // scroll: function(e) {
    //    this.scrollTop = e.detail.scrollTop
    // },
    sideBarHeadle(index) {
      this.scrollTop = this.goodListHeight[index];
      console.log(this.goodListHeight[index])
    }
  },
  onShow: function() {
    this.init();
    console.log(goodsData);
  },
  onReady: function() {
    this.setSideBarPoint();
    console.log(this.goodListHeight);
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
#scroll{
  height: 100vh;
}
</style>