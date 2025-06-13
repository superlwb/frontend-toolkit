<script setup>
import RequestButton from "@/components/RequestButton.vue";

const onRequest = async () => {
  const res = new Promise((resolve, reject) => {
    uni.showLoading({
      title: "请求等待5秒",
      // mask: true,
    });
    setTimeout(() => {
      uni.hideLoading()
      resolve("请求成功");
    }, 5000);
  });

  await res
};


const onRequestErr = async () => {
  const res = new Promise((resolve, reject) => {
    uni.showLoading({
      title: "请求等待5秒",
      // mask: true,
    });
    setTimeout(() => {
      uni.hideLoading()
      reject("请求失败");
    }, 5000);
  });

  await res
};
</script>

<template>
  <view class="page">
    <view>成功案例</view>
    <RequestButton :onRequest="onRequest">点击登录</RequestButton>
    <view>失败案例</view>
    <RequestButton :onRequest="onRequestErr"></RequestButton>

    请求按钮，传入一个promise函数，点击按钮后，会自动执行这个函数，并显示加载中，加载完成后，会自动隐藏加载中，如果请求失败，会显示失败
    使用方法： 
    1. 引入RequestButton组件 2. 在插槽中填入按钮名称 3. 传入一个使用async修饰的函数
      组件属性： onRequest：请求函数 width：按钮宽度
      插槽： default：按钮名称 error：请求失败时显示的文本
  </view>
</template>

<style lang="scss" scoped>
.page{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

}

</style>
