<script setup>
import { nextTick, onMounted, getCurrentInstance, ref, watch } from "vue";
const instance = getCurrentInstance();
const props = defineProps({
  show: {
    type: Boolean,
    default: false,
  },
  init: {
    type: Boolean,
    default: false,
  },
});

const emits = defineEmits(["update:show"]);

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
        if (props.init && props.show) {
          UIHeight.value = height.value;
        }

        // 如果init是false ， show是true，那更改show
        if (!props.init && props.show) {
          emits("update:show", false);
        }
        if (props.init && !props.show) {
          emits("update:show", true);
        }
      })
      .exec();
  });
});
</script>

<template>
  <view class="wrapper" :style="{ height: UIHeight }">
    <view class="func-list" :class="show ? 'show' : 'hidden'">
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
    transition: 0.3s opacity linear;
  }

  // 缩放时给元素增加透明过渡动画
  .show {
    opacity: 1;
  }

  .hidden {
    opacity: 0;
  }
}
</style>
