<template>
  <view class="func" @click="handleClick">
    <image :src="func.cover"></image>
    <view class="func-title">{{ func.title }}</view>
  </view>
</template>

<script setup>
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

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  > image {
    width: 60%;
    height: 60%;
    background-color: aquamarine;
  }

  .func-title {
    margin-top: 20upx;
    font-size: 30upx;
    color: #6e6c6c;
  }
}
</style>
