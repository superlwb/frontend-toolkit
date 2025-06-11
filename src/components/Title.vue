<template>
  <view class="title" :style="{ '--color-change-duration': colorChangeDuration + 's' }" @click="handleClick">
    <view
      v-for="(word, index) in title"
      :key="word"
      :class="`font-size-${size} margin-left-${marginLeft}`"
      :style="{ 'animation-delay': `${(index * colorChangeDuration) / 10}s` }"
    >
      {{ word }}
    </view>
    <image src="../static/downpush.png" :class="['down', show ? 'show' : '']" v-if="more"></image>
  </view>
</template>

<script setup>
import { ref, nextTick, onMounted, getCurrentInstance, defineEmits, watch } from "vue";
const instance = getCurrentInstance();

// 获取当前组件实例
const props = defineProps({
  title: {
    type: String,
    default: "",
  },
  more: {
    type: Boolean,
    default: false,
  },
  size: {
    type: Number,
    default: 50,
  },
  marginLeft: {
    type: Number,
    default: 20,
  },
  show: {
    type: Boolean,
    default: false,
  },
});
// const title = ref('前端百宝箱')
const colorChangeDuration = ref(3);
const emits = defineEmits(["pull"]);

const handleClick = () => {
  // console.log("aaa");
  emits("pull");
};
</script>

<style lang="scss" scoped>
.title {
  // font-size: 50upx;
  font-weight: bold;
  text-align: center;
  height: 200upx;
  line-height: 200upx;
  //animation: color-change 2s infinite alternate;
  --color-change-duration: 1s;
  width: 100%;
  background-color: #fff;
  border-radius: 10upx;
  padding: 20upx;
  border-radius: var(--radius);
  box-sizing: border-box;
  margin-bottom: var(--margin-bottom);
  box-shadow: var(--shadow);

  view {
    // margin-left: 20upx;
    color: #f07adf;
    display: inline-block;
    animation: color-change var(--color-change-duration) infinite alternate linear;
  }

  // @for $i from 1 through 10 {
  //   text:nth-child(#{$i}) {
  //     animation-delay: calc(var(--color-change-duration) * ($i / 10));
  //   }
  // }
}

.down {
  margin-left: 20upx;
  width: 30upx;
  height: 30upx;
  transition: 0.2s linear;
}

@keyframes color-change {
  0% {
    color: #f07adf;
  }
  100% {
    color: #a7ff0f;
    filter: hue-rotate(360deg);
  }
}

.show {
  transform: rotate(180deg);
}

.hidden {
  transform: rotate(0);
}

.font-size-50 {
  font-size: 50upx;
}

.font-size-35 {
  font-size: 35upx;
}

.font-size-25 {
  font-size: 25upx;
}

.margin-left-20 {
  margin-left: 20upx;
}

.margin-left-10 {
  margin-left: 10upx;
}

.margin-left-0 {
  margin-left: 0upx;
}
</style>
