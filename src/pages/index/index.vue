<template>
  <div class="video-container">
    <mTikTok
      ref="mTikTokRef"
      :video-list="state.videoList"
      @loadMore="loadMore"
      @change="change"
    >
      <!-- 此处为用户完全自定义 data 中的数据为当前渲染的数据 -->
      <template v-slot="data">
        <view class="video-layer"> 第{{ data.item.id }}个 </view>
      </template>
    </mTikTok>
  </div>
</template>
<script lang="ts" setup>
import { onMounted, reactive, ref } from "vue";
// 导入组件
import mTikTok from "@/components/mTikTok.vue";

const mTikTokRef = ref<InstanceType<typeof mTikTok>>();

const state = reactive({
  videoList: [
    {
      src: "https://xjc.demo.hongcd.com/uploads/20230214/84e165388f5bfdb1550522f50f5a57bb.mp4",
      id: "1",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20230214/3f26d950ac286eecedba49f5295f0819.mp4",
      id: "2",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20230215/8b5ac0420fe61e2f9660d7b8af998f7b.mp4",
      id: "3",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20210128/d932b2d78cebb0a8cb8f9a6216790dfb.mp4",
      id: "4",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20210128/0c64cbeea28b10c06eee8728c762449e.mp4",
      id: "5",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20210327/1b72e1b6153cd29df07f5449991e8083.mp4",
      id: "6",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20230214/7e1a0baaebc4e656bbbfbc44d7a55a60.mp4",
      id: "7",
    },
  ],
});

const loadMore = () => {
  // 触发加载更多
  console.log("加载更多");
};

const change = (e: any) => {
  console.log("🚀 ~ file: index.vue:53 ~ change ~ data:", e);
};

// 播放第几个
const playIndex = (index: number) => {
  mTikTokRef.value?.initSwiperData(index);
};

onMounted(() => {
  // 直接播放第3个
  playIndex(3);
});
</script>
<style lang="scss">
.video-layer {
  position: absolute;
  right: 12px;
  bottom: 120px;
  color: #fff;
}
</style>
