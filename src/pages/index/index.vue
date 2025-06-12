<template>
  <view class="page">
    <view class="block" v-for="(item, index) in titleList" :key="index">
      <Title
        :title="item.title"
        :size="item.size"
        :marginLeft="item.marginLeft"
        @pull="handlePull(index)"
        :more="item.more"
        :show="item.show"
      />
      <BlockDown :show="item.show" v-if="item.more" :init="item.init">
        <Func v-for="(func, index) in item.funcList" :func="func" :key="index" />
      </BlockDown>
    </view>
  </view>
</template>

<script setup>
import { reactive, ref } from "vue";
import { onLoad } from "@dcloudio/uni-app";
import Title from "@/components/Title.vue";
import BlockDown from "@/components/BlockDown.vue";
import Func from "@/components/Func.vue";
const list = [
  {
    title: "前端百宝箱",
    size: 50,
    marginLeft: 20,
    more: false,
    show: false,
    init: false,
    funcList: [],
  },
  {
    title: "uni-app优化小组件",
    size: 35,
    marginLeft: 0,
    more: true,
    show: true,
    init: true,
    funcList: [
      {
        cover: "https://superlwb0608.top/img/frontendTool/func/pullMenu.gif",
        title: "下拉菜单（无限制高度）",
        path: "/pages/function/UIComponents/pullMenu",
      },
      {
        cover: "https://superlwb0608.top/img/frontendTool/func/scroll.gif",
        title: "滚动加载组件",
        path: "/pages/function/UIComponents/refreshScrollView",
      },
      {
        cover: "https://superlwb0608.top/img/frontendTool/func/lazyImage.gif",
        title: "异步懒加载图片",
        path: "/pages/function/UIComponents/asyncLazyImage",
      },
      {
        cover: "",
        title: "新功能",
      },
    ],
  },
  {
    title: "开发者专用瑞士军刀",
    size: 35,
    marginLeft: 0,
    more: true,
    show: false,
    init: false,
    funcList: [
      {
        cover: "",
        title: "新功能",
      },
    ],
  },
  {
    title: "图片压缩与格式转换工具",
    size: 35,
    marginLeft: 0,
    more: true,
    show: false,
    init: false,
    funcList: [
      {
        cover: "",
        title: "新功能",
      },
    ],
  },
  {
    title: "在线证件照制作(换背景色)",
    size: 35,
    marginLeft: 0,
    more: true,
    show: false,
    init: false,
    funcList: [
      {
        cover: "",
        title: "新功能",
      },
    ],
  },
];

const titleList = reactive(list);

const handlePull = (index) => {
  // console.log("点击率");
  titleList[index].show = !titleList[index].show;
};

onLoad(() => {
  console.log("onLoad");
  console.log("启动了");
});
</script>

<style lang="scss" scoped>
.block {
  position: relative;
  --margin-bottom: 20upx;
  --radius: 10upx;
  --shadow: 0 0 6upx 0 rgba(0, 0, 0, 0.1);
}
</style>
