<script setup>
import LazyMedia from "@/components/LazyMedia.vue";
</script>
<template>
  <view class="page">
    <!-- 10m -->
    <LazyMedia :width="600" :height="400" unit="upx" src="https://superlwb0608.top/img/frontendTool/img/bigImage.png" />

    介绍： 该组件为图片懒加载，图片组件进入视口后才进行加载，同时有自定义加载动画、加载错误提示。 使用方法： 1.
    引入LazyImage组件 2. 配置 src 为图片地址 3. 配置 unit 图片大小单位 upx/px 4. 配置width大小 5. 配置height大小
    组件属性： src 图片地址、 unit 图片大小单位 、 width、height
  </view>
</template>

<style lang="scss" scoped>
.page {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
}
</style>
