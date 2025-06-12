<script setup>
import { ref, defineProps, computed, getCurrentInstance, onMounted, reactive } from "vue";
const instance = getCurrentInstance();
const observer = uni.createIntersectionObserver(instance, {
  thresholds: [0.1], // 当10%进入视口时触发
});

const props = defineProps({
  src: {
    type: String,
    default: "",
  },
  height: {
    type: Number,
    default: 400,
  },
  width: {
    type: Number,
    default: 300,
  },
  unit: {
    type: String,
    default: "upx",
  },
});

const onePx = uni.getSystemInfoSync().windowWidth / 750;
const imageSrc = ref("");
const isLoading = ref(true);
const isError = ref(false);

const comHeight = computed(() => {
  switch (props.unit) {
    case "upx":
      return onePx * props.height + "px";
    case "px":
      return props.height + props.unit;
  }
});

const comWidth = computed(() => {
  switch (props.unit) {
    case "upx":
      return onePx * props.width + "px";
    case "px":
      return props.width + props.unit;
  }
});

const handleLoad = (e) => {
  isLoading.value = false;
  // console.log("加载成功");
  observer.disconnect();
};

const handleError = () => {
  isLoading.value = false;
  isError.value = true;
  // console.log(isLoading.value);
};

onMounted(() => {
  // console.log(observer);
  observer
    .relativeToViewport({
      top: 100, // 提前100px加载
      bottom: 100,
    })
    .observe(".image-wrapper", (res) => {
      // console.log("视口", res.intersectionRatio > 0 && !props.src);
      // 如果进入视口，而且图片链接没有的情况下加载
      // intersectionRatio 目标元素可见区域占其自身大小的比例
      if (res.intersectionRatio > 0 && !imageSrc.value) {
        // 进入视口后设置真实图片地址
        imageSrc.value = props.src;
      }
    });
});
</script>

<template>
  <view class="image-wrapper" :style="{ '--width': comWidth, '--height': comHeight }">
    <view class="loading" :style="{ display: isLoading ? 'flex' : 'none' }">
      <image src="@/static/refresh.svg" mode="widthFix"></image>
    </view>
    <view class="image" :style="{ display: !isLoading && !isError ? 'flex' : 'none' }">
      <image :src="imageSrc" @load="handleLoad()" @error="handleError()" class="target-image"></image>
    </view>
    <view class="error" :style="{ display: isError ? 'flex' : 'none' }">图片加载错误</view>
  </view>
</template>

<style lang="scss" scoped>
.image-wrapper {
  --width: 300upx;
  --height: 400upx;
  width: var(--width);
  height: var(--height);
  // background-color: aqua;
  background-color: #fff;
  position: relative;

  .loading {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    top: 0%;
    left: 0%;
    z-index: 1;

    image {
      width: 50%;
      height: 50%;
      animation: rotate 1s infinite ease-in-out;
    }

    @keyframes rotate {
      to {
        transform: rotate(360deg);
      }
    }
  }
  .error {
    position: relative;
    width: 100%;
    height: 100%;
    background-color: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    color: red;
    opacity: 0.8;
    font-weight: 900;
    font-size: 30upx;
    z-index: 2;
  }

  .image {
    position: relative;
    z-index: 3;
    width: 100%;
    height: 100%;
    image {
      width: 100%;
      height: 100%;
    }
  }
}
</style>
