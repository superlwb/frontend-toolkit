<template>
  <view class="page">
    <button @click="handleClick">点击下拉</button>
    <BlockDown :show="show">
      <view class="item" v-for="(item, index) in 6" :key="index">
        内容{{ index + 1 }}
      </view>
    </BlockDown>

    介绍：
    该组件可以实现下拉菜单，且没有限制高度，可以自由设置内容。
    内容按grid布局，分为2列
    使用方法：
    1. 在页面中引入BlockDown组件
    2. 在页面中使用BlockDown组件
    3. 在BlockDown组件中使用item组件
    4. 在item组件中使用内容
    5. 在blockDown组件中使用show属性控制下拉菜单的显示

    组件属性：
    show：控制下拉菜单的显示
    
  </view>
</template>

<script setup>
import { ref } from "vue";
import BlockDown from "@/components/BlockDown.vue";
const show = ref(false);
const handleClick = () => {
  show.value = !show.value;
};
</script>

<style lang="scss" scoped>
.item {
  width: 100%;
  height: 200upx;
  background-color: #fff;
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
