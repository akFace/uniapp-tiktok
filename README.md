# 基于 uniapp 开发的仿抖音小程序组件（超高性能）

### GIF 截图效果

<figure class="center">
    <img src="https://github.com/akFace/tool/blob/master/video.gif?raw=true">
</figure>

- GIF 帧率不够，实际真机效果非常丝滑

### 注：组件使用 vue3+typescript 开发

- 全局仅渲染 3 个 swiper-item
- 实测，不管加载多少数据也能丝滑滚动
- 适用于 vue3，vue2 请自行修
- 自动预加载视频
- 首次渲染优化

## 快速开始，下载插件后请按照此方法运行调试

1. 安装 nodejs: https://nodejs.org/en/
2. 安装依赖: `npm i`
3. 运行项目: `npm run dev:mp-weixin`
4. 构建项目资源: `npm run build:mp-weixin`
5. 打开小程序开发工具导入`dist/dev/mp-weixin` 即可
6. 真机预览，请点小程序开发工具上的预览，扫码真机预览即可

## 参考 API

| 属性                | 类型        | 默认值 | 说明                                                                                                    |
| ------------------- | ----------- | ------ | ------------------------------------------------------------------------------------------------------- |
| videoList           | Array       | -      | 视频列表，数组对象 `{src: string, poster?: string, objectFit?: string}`                                 |
| autoObjectFit       | Boolean     | true   | 是否开启视频自动自适应平铺模式，竖屏全屏覆盖，横屏自适应居中, 此参数优先级低于 videoList 中的 objectFit |
| loop                | Boolean     | true   | 是否循环播放视频                                                                                        |
| controls            | Boolean     | false  | 显示原生控制栏                                                                                          |
| autoplay            | Boolean     | true   | 是否自动播放                                                                                            |
| autoChange          | Boolean     | false  | 是否自动滚动播放                                                                                        |
| loadMoreOffsetCount | Number      | 2      | 滚动加载阈值（即播放到剩余多少个之后触发加载更多                                                        |
| @play               | EventHandle | -      | 当开始/继续播放时触发 play 事件                                                                         |
| @error              | EventHandle | -      | 视频播放出错时触发                                                                                      |
| @ended              | EventHandle | -      | 当播放到末尾时触发 ended 事件                                                                           |
| @loadMore           | EventHandle | -      | 当滚动到最后第 N 条数据后，需要加载更多时触发                                                           |
| @change             | EventHandle | -      | 切换视频时触发                                                                                          |
| @click              | EventHandle | -      | 点击整个视频区域触发                                                                                    |
| @controlstoggle     | EventHandle | -      | 控制栏状态变化触发                                                                                      |

### Slots 插槽

| 属性    | 默认值 | 说明                                                                                    |
| ------- | ------ | --------------------------------------------------------------------------------------- |
| default | -      | 自定义内容，覆盖到视频上方的所有自定义内容 v-slot="data" 为当前渲染数据，请参照使用示例 |

### 方法

```javascript
// 播放第几个视频
mTikTokRef.value?.initSwiperData(index);

// 播放与暂停
mTikTokRef.value?.togglePlay();

// 播放跳转到指定位置，单位 s
mTikTokRef.value?.playSeeked(8);
```

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
        <!-- active修复视频悬浮层消失和点击的问题 -->
        <view
          class="video-side-right"
          :class="{ active: state.cutVideo.id === data.item.id }"
        >
          <view class="action-item action-item-user">
            <image
              class="shop-logo"
              src="https://examples-1251000004.cos.ap-shanghai.myqcloud.com/sample.jpeg?imageMogr2/crop/180x180/gravity/center"
            />
            <view class="action-btn">
              <text class="iconfont">+</text>
            </view>
            <text class="action-item-text"></text>
          </view>
          <view class="action-item">
            <text class="iconfont icon-star11beifen">❤</text>
            <text class="action-item-text">{{ data.item.fav }}</text>
          </view>
          <view class="action-item">
            <text class="iconfont icon-share">☝</text>
            <text class="action-item-text">分享</text>
          </view>
        </view>
        <!-- active修复视频悬浮层消失和点击的问题 -->
        <view
          class="video-bottom-area"
          :class="{ active: state.cutVideo.id === data.item.id }"
        >
          <view class="shop-name"> @{{ data.item.name }} </view>
          <view class="shop-card">{{ data.item.desc }}</view>
        </view>
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
  cutVideo: {} as AnyObject,
  videoList: [
    {
      src: "https://sns-video-hw.xhscdn.com/stream/1/110/258/01e8dda817e899370103700199a1dfa9b2_258.mp4",
      id: "1",
      name: "开玩笑的鸡毛",
      fav: 154,
      desc: "这里是简介内容",
    },
    {
      src: "https://sns-video-hw.xhscdn.com/stream/79/110/258/01e8ef3a5e7fd48f4f03700199e67c96fa_258.mp4",
      id: "2",
      name: "开玩笑的鸡毛",
      fav: 1544,
      desc: "这里是简介内容",
    },
    {
      src: "https://sns-video-hw.xhscdn.com/stream/79/110/258/01e8e5fa652aa89b4f03700199c25a5e16_258.mp4",
      id: "3",
      name: "开玩笑的鸡毛",
      fav: 457,
      desc: "这里是简介内容",
    },
    {
      src: "https://sns-video-hw.xhscdn.com/stream/1/110/258/01e8ec727be8bc9a0103700199dba00279_258.mp4",
      id: "4",
      name: "开玩笑的鸡毛",
      fav: 44,
      desc: "这里是简介内容",
    },
    {
      src: "https://sns-video-hw.xhscdn.com/stream/79/110/258/01e8dca9ade7eb494f037001999df7cd33_258.mp4",
      id: "5",
      name: "开玩笑的鸡毛",
      fav: 4154,
      desc: "这里是简介内容",
    },
    {
      src: "https://sns-video-hw.xhscdn.com/stream/1/110/258/01e8e24eace7c4880103700199b4049ff4_258.mp4",
      id: "6",
      name: "开玩笑的鸡毛",
      fav: 1054,
      desc: "这里是简介内容",
    },
    {
      src: "https://sns-video-hw.xhscdn.com/stream/79/110/258/01e8de25bddb4b714f03700199a3c3a989_258.mp4",
      id: "7",
      name: "开玩笑的鸡毛",
      fav: 8154,
      desc: "这里是简介内容",
    },
  ],
});

const loadMore = () => {
  // 触发加载更多
  console.log("加载更多");
};

const change = (e: any) => {
  state.cutVideo = e.detail;
  console.log("🚀 ~ file: index.vue:53 ~ change ~ data:", e);
};

// 播放第几个
const playIndex = (index: number) => {
  mTikTokRef.value?.initSwiperData(index);
};

onMounted(() => {
  // 直接播放第3个
  // playIndex(3);
});
</script>
<style lang="scss">
$zIndex: 99;

.video-layer {
  position: absolute;
  right: 12px;
  bottom: 120px;
  color: #fff;
}

.video-bottom-area {
  position: absolute;
  left: 20px;
  bottom: 40px;
  opacity: 0;
  transition: all 250ms;
  z-index: 0;

  &.active {
    opacity: 1;
    z-index: $zIndex;
    transition-delay: 200ms;
  }

  .shop-name {
    color: #fff;
    margin-bottom: 6px;
  }

  .shop-card {
    width: 160px;
    height: 80px;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 4px;
  }
}

.video-side-right {
  position: absolute;
  right: 12px;
  bottom: 120px;
  color: #fff;
  opacity: 0;
  transition: all 250ms;
  z-index: 0;

  &.active {
    opacity: 1;
    z-index: $zIndex;
    transition-delay: 200ms;
  }

  .action-item {
    position: relative;
    margin-bottom: 20px;
    text-align: center;

    .shop-logo {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      overflow: hidden;
    }

    .iconfont {
      display: block;
      font-size: 28px;
    }

    .action-item-text {
      display: block;
      font-size: 12px;
    }

    .action-btn {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      bottom: -8px;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: #f60;

      .iconfont {
        font-size: 16px;
      }
    }
  }

  .action-item-user {
    margin-bottom: 40px;
  }
}
</style>


```
