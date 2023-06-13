<template>
  <swiper
    class="m-tiktok-video-swiper"
    circular
    @change="swiperChange"
    :current="state.current"
    :vertical="true"
    duration="300"
  >
    <swiper-item v-for="(item, index) in state.displaySwiperList" :key="index">
      <view class="swiper-item" @click="handleClick">
        <video
          :id="`video__${index}`"
          :controls="controls"
          :autoplay="false"
          :loop="loop"
          :object-fit="item.objectFit"
          @ended="ended"
          @controlstoggle="controlstoggle"
          @play="onPlay"
          @error="emits('error')"
          class="m-tiktok-video-player"
          :src="item.src"
          v-if="index === 0 || !state.isFirstLoad"
        ></video>
        <image
          v-if="item.poster && state.displayIndex != index"
          :src="item.poster"
          class="m-tiktok-video-poster"
          mode="aspectFit"
        ></image>
        <slot :item="item"></slot>
      </view>
    </swiper-item>
  </swiper>
</template>

<script lang="ts" setup>
import { reactive, getCurrentInstance, watch } from "vue";
import type { ComponentInternalInstance, PropType } from "vue";
import { onLoad, onUnload } from "@dcloudio/uni-app";
const _this = getCurrentInstance() as ComponentInternalInstance;

export interface IvideoItem {
  /**
   * 视频链接
   */
  src: string;
  /**
   * 海报封面
   */
  poster?: string;
  /**
   * 当视频大小与 video 容器大小不一致时，视频的表现形式。contain：包含，fill：填充，cover：覆盖
   */
  objectFit?: string;
}

const emits = defineEmits([
  "play",
  "error",
  "loadMore",
  "change",
  "controlstoggle",
  "click",
  "ended",
]);

const props = defineProps({
  /**
   * 视频列表
   */
  videoList: {
    type: Array as PropType<IvideoItem[]>,
    default: () => [],
  },
  /**
   * 是否循环播放一个视频
   */
  loop: {
    type: Boolean,
    default: true,
  },
  /**
   * 显示原生控制栏
   */
  controls: {
    type: Boolean,
    default: true,
  },
  /**
   * 是否自动播放
   */
  autoplay: {
    type: Boolean,
    default: true,
  },
  /**
   * 是否自动滚动播放
   */
  autoChange: {
    type: Boolean,
    default: false,
  },
  /**
   * 滚动加载阈值（即播放到剩余多少个之后触发加载更多
   */
  loadMoreOffsetCount: {
    type: Number,
    default: 2,
  },
});

const state = reactive({
  originList: [] as any, // 源数据
  displaySwiperList: [] as any, // swiper需要的数据
  displayIndex: 0, // 用于显示swiper的真正的下标数值只有：0，1，2。
  originIndex: 0, // 记录源数据的下标
  current: 0,
  oid: 0,
  showControls: "",
  toggleShow: true, // 显示面板
  videoContexts: [] as any,
  isFirstLoad: true,
});

const initVideoContexts = () => {
  state.videoContexts = [
    uni.createVideoContext("video__0", _this),
    uni.createVideoContext("video__1", _this),
    uni.createVideoContext("video__2", _this),
  ];
};

const onPlay = (e: Event) => {
  emits("play", e);
};

function handleClick(e: Event) {
  state.toggleShow = !state.toggleShow;
  emits("click", e);
}
function ended() {
  // 自动切换下一个视频
  if (props.autoChange) {
    if (state.displayIndex < 2) {
      state.current = state.displayIndex + 1;
    } else {
      state.current = 0;
    }
  }
  emits("ended");
}
/**
 * 初始一个显示的swiper数据
 * @originIndex  从源数据的哪个开始显示默认0，如从其他页面跳转进来，要显示第n个，这个参数就是他的下标
 */
function initSwiperData(originIndex = state.originIndex) {
  const originListLength = state.originList.length; // 源数据长度
  const displayList = [];
  displayList[state.displayIndex] = state.originList[originIndex];
  displayList[state.displayIndex - 1 == -1 ? 2 : state.displayIndex - 1] =
    state.originList[
      originIndex - 1 == -1 ? originListLength - 1 : originIndex - 1
    ];
  displayList[state.displayIndex + 1 == 3 ? 0 : state.displayIndex + 1] =
    state.originList[originIndex + 1 == originListLength ? 0 : originIndex + 1];
  state.displaySwiperList = displayList;

  if (state.oid >= state.originList.length) {
    state.oid = 0;
  }
  if (state.oid < 0) {
    state.oid = state.originList.length - 1;
  }
  // 暂停所有视频
  state.videoContexts.map((item: any) => item?.stop());
  setTimeout(() => {
    // 当前视频
    if (props.autoplay) {
      uni.createVideoContext(`video__${state.displayIndex}`, _this).play();
    }
  }, 500);
  // 数据改变
  emits("change", {
    index: originIndex,
    detail: state.originList[originIndex],
  });
  // 加载更多
  var pCount = state.originList.length - props.loadMoreOffsetCount;
  if (originIndex == pCount) {
    emits("loadMore");
  }
}
/**
 * swiper滑动时候
 */
function swiperChange(event: any) {
  const { current } = event.detail;
  state.isFirstLoad = false;
  const originListLength = state.originList.length; // 源数据长度
  // 向后滚动
  if (state.displayIndex - current == 2 || state.displayIndex - current == -1) {
    state.originIndex =
      state.originIndex + 1 == originListLength ? 0 : state.originIndex + 1;
    state.displayIndex =
      state.displayIndex + 1 == 3 ? 0 : state.displayIndex + 1;
    state.oid = state.originIndex - 1;
    initSwiperData(state.originIndex);
  }
  // 如果两者的差为-2或者1则是向前滑动
  else if (
    state.displayIndex - current == -2 ||
    state.displayIndex - current == 1
  ) {
    state.originIndex =
      state.originIndex - 1 == -1
        ? originListLength - 1
        : state.originIndex - 1;
    state.displayIndex =
      state.displayIndex - 1 == -1 ? 2 : state.displayIndex - 1;
    state.oid = state.originIndex + 1;
    initSwiperData(state.originIndex);
  }
  state.toggleShow = true;
}

function controlstoggle(e: any) {
  state.showControls = e.detail.show;
  emits("controlstoggle", e);
}

watch(
  () => props.videoList,
  () => {
    if (props.videoList?.length) {
      state.originList = props.videoList;
      if (state.isFirstLoad || !state.videoContexts?.length) {
        initSwiperData();
        initVideoContexts();
      }
    }
  },
  {
    immediate: true,
  }
);

let loadTimer: any = null;
onLoad(() => {
  // 为了首次只加载一条视频（提高首次加载性能），延迟加载后续视频
  loadTimer = setTimeout(() => {
    state.isFirstLoad = false;
    clearTimeout(loadTimer);
  }, 5000);
});

onUnload(() => {
  clearTimeout(loadTimer);
});

defineExpose({
  initSwiperData,
});
</script>

<style lang="scss">
.m-tiktok-video-swiper,
.m-tiktok-video-player {
  width: 100%;
  height: 100vh;
  background-color: #000;
}
.m-tiktok-video-swiper {
  .swiper-item {
    position: relative;
  }
  .m-tiktok-video-poster {
    background-color: #000;
    position: absolute;
    width: 100%;
    height: 100%;
  }
}
</style>
