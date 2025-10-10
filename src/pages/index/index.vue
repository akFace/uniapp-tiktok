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
            <text class="action-item-text">{{ data.item.id }}</text>
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
      src: "https://v26-default.365yg.com/3bc662e29e2c5a564daa14ed0881a3bd/68e8a0f9/video/tos/cn/tos-cn-ve-15/osmBdI0GA4cCF7PLeeR7BBAcgBfUrvR9vLYGLG/",
      id: "1",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://v26-default.365yg.com/b5ca75db19cfad6c550a7cae87042d27/68e8a1f7/video/tos/cn/tos-cn-ve-15c000-ce/oMMkDB7PbNJXQC1aXVEXCAi2vvhPIhP48iREA/?a=0&ch=0&cr=0&dr=0&lr=unwatermarked",
      id: "2",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://v26-default.365yg.com/7edfb2f43cd31206a70369e4909dc265/68e8a2b4/video/tos/cn/tos-cn-ve-15/o8iKjPTPiQoidBIAQdq7AQb4pahE8ZNvItxja/?a=0&ch=0&cr=0&dr=0&lr=unwatermarked",
      id: "3",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://v3-default.365yg.com/e5615180d069b9348a46c62840b8fdac/68e8a359/video/tos/cn/tos-cn-ve-15/oE3B3aG3L2hNzARoS7feACrBD06QIGheARxx8I/?a=0&ch=0&cr=0&dr=0&lr=unwatermarked",
      id: "4",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://v9-default.365yg.com/1e9378f938cd86c380309b11d941854f/68e8a34e/video/tos/cn/tos-cn-ve-15c000-ce/oQB4czXcLQPibCESWDP3rIAAIEaNPUizKhm9M/?a=0&ch=0&cr=0&dr=0&lr=unwatermarked",
      id: "5",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://v11-default.365yg.com/327db7dd13b4be61f9ae1c39249195ca/68e8a3be/video/tos/cn/tos-cn-ve-15/o0KngYi5TAAkg9HogQFovGNDCfFSjsZEAABVfI/?a=0&ch=0&cr=0&dr=0&lr=unwatermarked",
      id: "6",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://v11-default.365yg.com/3d5c806cd0664f32fbed54fd29f66b3b/68e8a416/video/tos/cn/tos-cn-ve-15c001-alinc2/oQI85kNAaBRIhg9zQfEAWeDAoIn91uQF0bpu8D/?a=0&ch=0&cr=0&dr=0&lr=unwatermarked",
      id: "7",
      name: "开玩笑的鸡毛",
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
