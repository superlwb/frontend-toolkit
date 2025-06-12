<template>
  <view class="func" @click="handleClick">
    <!-- <image :src="func.cover"></image> -->
    <lazy-image :src="func.cover" height="250" width="200" unit="upx" />
    <view class="func-title">{{ func.title }}</view>
  </view>
</template>

<script setup>
import LazyImage from "@/components/LazyImage.vue";
const props = defineProps({
  func: {
    type: Object,
    default: {
      cover: "",
      title: "",
      path: "",
    },
  },
});

const handleClick = () => {
  uni.navigateTo({
    url: props.func.path,
  });
};
</script>

<style lang="scss" scoped>
.func {
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  width: 100%;
  height: 400upx;
  background-color: #fff;
  position: relative;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  .func-title {
    margin-top: 20upx;
    font-size: 30upx;
    color: #6e6c6c;
  }
}
</style>
