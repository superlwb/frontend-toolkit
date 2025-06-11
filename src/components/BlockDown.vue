<script setup>
import { nextTick, onMounted, getCurrentInstance, ref, watch } from "vue";
const instance = getCurrentInstance();
const props = defineProps({
  show: {
    type: Boolean,
    default: false,
  },
});

const height = ref("0");
const UIHeight = ref("0");

watch(
  () => props.show,
  async (n, o) => {
    if (n) {
      UIHeight.value = height.value;
    } else {
      UIHeight.value = 0;
    }
  },
  {
    immediate: true,
  }
);

onMounted(() => {
  // getHeight();
  nextTick(() => {
    const query = uni.createSelectorQuery().in(instance);
    query
      .select(".func-list")
      .boundingClientRect((data) => {
        height.value = data.height + "px";
      })
      .exec();
  });
});
</script>

<template>
  <view class="wrapper" :style="{ height: UIHeight }">
    <view class="func-list">
      <slot></slot>
    </view>
  </view>
</template>

<style lang="scss" scoped>
.wrapper {
  // position: absolute;
  // bottom: 0%;
  width: 100%;
  // height: 400upx;
  // background-color: aqua;
  margin-bottom: var(--margin-bottom);
  border-radius: var(--radius);
  overflow: hidden;
  transition: 0.5s height ease-out;
  will-change: height; /* 优化动画性能 */

  .func-list {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--margin-bottom);
  }
}
</style>
