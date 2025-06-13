<script setup>
import { ref,computed } from "vue";
const loading = ref(false);
const error = ref(false);

const props = defineProps({
  onRequest: {
    type: Function,
    required: true,
  },

  width: {
    type: String,
    default: "250",
  }
});

const onePx = uni.getWindowInfo().windowWidth / 750;

const computedWidth = computed(() => {
  return onePx * +props.width  + 'px'
})

const handleClick = async () => {
  if (loading.value) {
    uni.showToast({
      title: "加载中，勿重复点击",
      icon: "none",
    });
    return;
  }
  loading.value = true;

  try {
   const res = props.onRequest()
   // 等待 promise的这个函数 执行完成
   if(res.then)  {
    await res
   }
  } catch (err) {
    error.value = true
    console.warn(err)
  }finally{
    loading.value = false
  }
  
};
</script>
<template>
  <view class="button-wrapper" :class="loading ? 'bg-change' : ''" @click="handleClick" :style="{width: computedWidth}">
    <view class="default" :class="loading ? 'loaded' : !error ? 'isLoading' : 'loaded'">
      <slot name="default">
        点击加载
      </slot>
    </view>
    <view class="loading" :class="!loading ? 'loaded' : 'isLoading'">
      <image src="@/static/loading.svg"></image>
      <view class="text">加载中</view>
      <view class="point-wrapper">
        <view class="point">.</view>
        <view class="point">.</view>
        <view class="point">.</view>
      </view>
    </view>

    <!-- false ,true -->
    <view class="error" :class="loading ? 'loaded' : error ? 'isLoading' : 'loaded'">
      <slot name="error">请求失败</slot>
    </view>
  </view>
</template>

<style lang="scss" scoped>
.button-wrapper {
  --width: 250upx;
  padding: 20upx 30upx;
  margin: 15upx;
  display: inline-flex;
  width: var(--width);
  height: 50upx;
  justify-content: center;
  align-items: center;
  border-radius: 50upx;
  background: linear-gradient(to right, #4361ee, #4cc9f0, #89cff0);
  background-size: 200% 200%;
  font-size: 35upx;
  letter-spacing: 5upx;
  color: #fff;
  will-change: animation;
  position: relative;
}

.bg-change {
  animation: change 3s linear infinite;
}

@keyframes change {
  0% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.default,
.error,
.loading {
  position: absolute;
  transition: 1s linear opacity;
}

.error{
  color: rgb(180, 178, 178);
}

.loading {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;

  image {
    width: 40upx;
    height: 40upx;
    animation: rotate 1s linear infinite;
    margin-right: 10upx;
  }

  .point-wrapper {
    margin-left: 5upx;
    display: flex;
    justify-content: center;
    align-items: center;

    .point {
      opacity: 0;
      animation: show 1s linear infinite;
    }
  }

  @keyframes rotate {
    100% {
      transform: rotate(360deg);
    }
  }

  @keyframes show {
    100% {
      opacity: 1;
    }
  }
}

.isLoading {
  opacity: 1;
}

.loaded {
  opacity: 0;
}
</style>
