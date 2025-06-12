<template>
  <view class="page">
    <scroll-view
      class="scroll-view"
      refresher-enabled
      refresher-default-style="none"
      :refresher-triggered="isRefreshing"
      :refresher-threshold="refreshHeight"
      scroll-y
      @refresherrefresh="handleRefresherrefresh"
      @scrolltoupper="handleScrollToUpper"
      @scroll="handleScroll"
      @scrolltolower="handleScrollToLower"
      :style="{ '--height': height + unit }"
    >
      <template slot="refresher">
        <view class="refresher-wrapper" :style="{ '--refresher-height': refreshHeight + 'px' }">
          <view class="refresher-icon">
            <image src="../../../static/refresh.svg" :class="{ rotate: isRefreshing }"></image>
          </view>
          <view class="refresher-text">
            <text v-if="!isRefreshing">下拉加载数据</text>
            <text v-else>加载中...</text>
          </view>
        </view>
      </template>

      <view class="data-list" :style="{ '--scroll-height': scrollHeight + 'px' }">
        <view class="data-list-inner" :style="{ transform: ' translate3d(0,' + offsetY + 'px,0)' }">
          <view class="item" v-for="(item, index) in uiDataList" :key="item.id" :style="{ height: dataHeight + 'px' }">
            内容{{ item.name }}{{ item.id }}
          </view>

          <view class="push-add-item" :style="{ opacity: isLoading ? 0 : 1, height: dataHeight + 'px' }">
            <view class="push-add-item-text">下拉加载更多...</view>
          </view>
        </view>
      </view>
    </scroll-view>

    <view>
      基于原scroll-view添加虚拟滚动 使用方法： 1. 引入VirtualScroll组件 2. 填入内容高度（高度依赖） 3. 编辑item内容
      组件属性： refreshHeightUpx：上拉刷新高度，单位upx height：scroll-view高度unit：scroll-view单位
    </view>
  </view>
</template>

<script setup>
import { ref, computed } from "vue";

// 获取页面px
const onePx = uni.getSystemInfoSync().windowWidth / 750;

const data = [
  {
    id: 1,
    name: "张三",
    age: 18,
  },
  {
    id: 2,
    name: "李四",
    age: 20,
  },
  {
    id: 3,
    name: "王五",
    age: 22,
  },
  {
    id: 4,
    name: "赵六",
    age: 24,
  },
  {
    id: 5,
    name: "孙七",
    age: 26,
  },
  {
    id: 6,
    name: "周八",
    age: 28,
  },
];

const props = defineProps({
  refreshHeightUpx: {
    type: Number,
    default: 200,
  },
  height: {
    type: Number,
    default: 60,
  },
  unit: {
    type: String,
    default: "vh",
  },
  // data:{
  //   type:Array,
  //   default:[]
  // }
});

const dataList = ref([]);
const dataCount = ref(100);
const viewCount = ref(20);
const start = ref(0);
const end = ref(viewCount.value);
// 缓冲个数
const bufCount = 5;
const offsetY = ref(0);

// 这里的100是upx
const dataHeight = 100 * onePx;

const addTestData = function (count) {
  const arr = new Array(count).fill({}).map((v, i) => ({
    name: "我是虚拟滚动内容",
    id: i + 1,
  }));
  dataList.value = arr;
};

addTestData(dataCount.value);

const isRefreshing = ref(false);
const isLoading = ref(false);

const refreshHeight = computed(() => {
  return props.refreshHeightUpx * onePx;
});

const scrollHeight = computed(() => {
  return dataHeight * dataList.value.length;
});

const uiDataList = computed(() => {
  return dataList.value.slice(start.value, end.value);
});

const handleRefresherrefresh = () => {
  uni.showLoading({
    title: "正在刷新",
  });
  isRefreshing.value = true;
  setTimeout(() => {
    isRefreshing.value = false;
    uni.hideLoading();
  }, 1000);
};

const handleScrollToUpper = () => {
  console.log("滚动到顶部");
};

// 节流
const throttle = (fn, delay) => {
  let timer = null;
  return function (...args) {
    if (!timer) {
      timer = setTimeout(() => {
        fn.apply(this, args);
        timer = null;
      }, delay);
    }
  };
};

const handleScrollToLower = throttle(() => {
  // console.log("滚动到底部");
  uni.showLoading({
    title: "正在加载",
  });
  isLoading.value = true;
  setTimeout(() => {
    dataList.value.push(...data);

    isLoading.value = false;
    uni.hideLoading();
  }, 1000);
}, 100);

let timer = null;

const handleScroll = (e) => {
  // console.log(e.detail.scrollTop);
  // 计算滚动个数
  if (timer) clearTimeout(timer);
  timer = setTimeout(() => {
    const offset = e.detail.scrollTop;
    const startIndex = Math.round(offset / dataHeight);
    const endIndex = startIndex + viewCount.value + bufCount;
    start.value = Math.max(0, startIndex - bufCount);
    end.value = Math.min(dataList.value.length, endIndex);
    offsetY.value = Math.max(0, offset - bufCount * dataHeight);
    // console.log(startIndex, "个数");
  }, 16);
};
</script>

<style lang="scss" scoped>
.item {
  width: 95%;
  margin: 0upx auto;
  // height: 100upx;
  // background-color: aqua;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10upx;
  box-sizing: border-box;
  background-color: #fff;
}


.data-list{
  --scroll-height:100%
  width: 100%;
  height: var(--scroll-height);

  .data-list-inner{
    will-change: transform;
  }
}

.push-add-item {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  // background-color: red;
  font-size: 30upx;
  letter-spacing: 10upx;
  color: #999;
}

.refresher-wrapper {
  --refresher-height: 100upx;
  width: 750upx;
  height: var(--refresher-height);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30upx;
  box-shadow: 10upx 5upx 10upx 0 rgba(0, 0, 0, 0.1);
  margin-bottom: 20upx;
  color: #999;

  .refresher-text {
    letter-spacing: 10upx;
  }

  .refresher-icon {
    width: 50upx;
    height: 50upx;
    // background-color: #fff;
    margin-right: 20upx;

    image {
      width: 100%;
      height: 100%;
    }
  }

  .rotate {
    animation: rotate 1s linear infinite;
  }

  @keyframes rotate {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
}

.scroll-view {
  box-sizing: border-box;
  padding: 10upx;
  --height: 100vh;
  width: 100%;
  // min-height: 100vh;
  height: var(--height);
  overflow-y: hidden;
  // position: relative;
  // z-index: 2;
  background-color: rgba(0, 0, 0, 0.01);
  // position: absolute;
  // top: 0upx;
  // left: 0upx;
  transform: translate3d(0, 0, 0);
  // transform: translateY(0upx);
  // transition: transform 0.5s linear;

}
</style>
