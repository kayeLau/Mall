<template>
    <view>
        <swiper indicator-dots autopaly :interval='3000' :duration='1000' circilar>
            <swiper-item v-for='(item,index) in swiperData' :key="index">
                <view class="swipter-item">
                    <image :src="item.src"
                    lazy-load
                    style='height:350rpx'></image>
                </view>
            </swiper-item>
        </swiper>
    </view>
</template>
<script>
export default {
    props:{
        swiperData:Array
    }
}
</script>