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
        <view class="video-side-right">
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
        <view class="video-bottom-area">
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
  videoList: [
    {
      src: "https://xjc.demo.hongcd.com/uploads/20230214/84e165388f5bfdb1550522f50f5a57bb.mp4",
      id: "1",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20230214/3f26d950ac286eecedba49f5295f0819.mp4",
      id: "2",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20230215/8b5ac0420fe61e2f9660d7b8af998f7b.mp4",
      id: "3",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20210128/d932b2d78cebb0a8cb8f9a6216790dfb.mp4",
      id: "4",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20210128/0c64cbeea28b10c06eee8728c762449e.mp4",
      id: "5",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20210327/1b72e1b6153cd29df07f5449991e8083.mp4",
      id: "6",
      name: "开玩笑的鸡毛",
      desc: "这里是简介内容",
    },
    {
      src: "https://xjc.demo.hongcd.com/uploads/20230214/7e1a0baaebc4e656bbbfbc44d7a55a60.mp4",
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
  z-index: 999;
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
  z-index: 999;
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
