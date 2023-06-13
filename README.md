# 基于 uniapp 开发的仿抖音小程序组件（超高性能）

### 注：组件使用 vue3+typescript 开发

- 全局仅渲染 3 个 swiper-item
- 实测，不管加载多少数据也能丝滑滚动
- 适用于 vue3，vue2 请自行修

## 快速开始

1. 安装 nodejs: https://nodejs.org/en/
2. 安装依赖: `npm i`
3. 运行项目: `npm run dev:mp-weixin`
4. 构建项目资源: `npm run build:mp-weixin`
5. 打开小程序开发工具导入`dist/dev/mp-weixin` 即可

## 参考 API

| 属性                | 类型        | 说明                                                                   |
| ------------------- | ----------- | ---------------------------------------------------------------------- |
| videoList           | Array       | 视频列表，数组对象 `{src: string, poster?: string, objectFit: string}` |
| loop                | Boolean     | 是否循环播放视频                                                       |
| controls            | Boolean     | 显示原生控制栏                                                         |
| autoplay            | Boolean     | 是否自动播放                                                           |
| autoChange          | Boolean     | 是否自动滚动播放                                                       |
| loadMoreOffsetCount | Boolean     | 滚动加载阈值（即播放到剩余多少个之后触发加载更多                       |
| @play               | EventHandle | 当开始/继续播放时触发 play 事件                                        |
| @error              | EventHandle | 视频播放出错时触发                                                     |
| @ended              | EventHandle | 当播放到末尾时触发 ended 事件                                          |
| @loadMore           | EventHandle | 当滚动到最后第 N 条数据后，需要加载更多时触发                          |
| @change             | EventHandle | 切换视频时触发                                                         |
| @click              | EventHandle | 点击整个视频区域触发                                                   |
| @controlstoggle     | EventHandle | 控制栏状态变化触发                                                     |

### Slots 插槽

| 属性    | 默认值 | 说明                                                                                    |
| ------- | ------ | --------------------------------------------------------------------------------------- |
| default | -      | 自定义内容，覆盖到视频上方的所有自定义内容 v-slot="data" 为当前渲染数据，请参照使用示例 |

## 使用示例

```javascript

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

```
