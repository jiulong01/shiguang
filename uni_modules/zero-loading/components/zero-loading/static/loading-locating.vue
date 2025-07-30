<template>
    <view class="animations">
      <view class="loader" :style="{ '--color': color }"></view>
    </view>
  </template>
  
  <script>
  export default {
    name: "loading-locating",
    props: {
      color: {
        type: String,
        default: "#0396FF",
      },
    },
    data() {
      return {};
    },
  };
  </script>
  
  <style lang="scss" scoped>
  
  .loader {
    width: 96rpx;
    height: 96rpx;
    display: block;
    margin: 40rpx auto;
    box-sizing: border-box;
    position: relative;
  }

  .loader::after {
    content: '';
    width: 96rpx;
    height: 96rpx;
    left: 0;
    bottom: 0;
    position: absolute;
    border-radius: 50% 50% 0;
    border: 30rpx solid var(--color);
    transform: rotate(45deg) translate(0, 0);
    box-sizing: border-box;
    animation: animMarker 0.4s ease-in-out infinite alternate;
  }

  .loader::before {
    content: '';
    box-sizing: border-box;
    position: absolute;
    left: 0;
    right: 0;
    margin: auto;
    top: 150%;
    width: 48rpx;
    height: 8rpx;
    border-radius: 50%;
    background: rgba(0, 0, 0, 0.2);
    animation: animShadow 0.4s ease-in-out infinite alternate;
  }

  @keyframes animMarker {
    0% {
      transform: rotate(45deg) translate(10rpx, 10rpx);
    }

    100% {
      transform: rotate(45deg) translate(-10rpx, -10rpx);
    }
  }

  @keyframes animShadow {
    0% {
      transform: scale(0.5);
    }

    100% {
      transform: scale(1);
    }
  }
</style>