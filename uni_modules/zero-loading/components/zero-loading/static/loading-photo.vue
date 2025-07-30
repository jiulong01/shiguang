<template>
  <view class="animations">
    <view class="loader" :style="{ '--color': color }"></view>
  </view>
</template>

<script>
export default {
  name: "loading-photo",
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
  width: 128rpx;
  height: 128rpx;
  position: relative;
  background: #f4f4f4;
  border-radius: 8rpx;
  overflow: hidden;
}

.loader:before {
  content: "";
  position: absolute;
  left: 0;
  bottom: 0;
  width: 80rpx; 
  height: 80rpx; 
  transform: rotate(45deg) translate(30%, 40%);
  background: var(--color);
  box-shadow: 64rpx -68rpx 0 10rpx var(--color); 
  animation: slide 2s infinite ease-in-out alternate;
}

.loader:after {
  content: "";
  position: absolute;
  left: 20rpx; 
  top: 20rpx; 
  width: 28rpx; 
  height: 28rpx; 
  border-radius: 50%;
  background: var(--color);
  transform: rotate(0deg);
  transform-origin: 70rpx 290rpx; 
  animation: rotate 2s infinite ease-in-out;
}

@keyframes slide {
  0% , 100% {
    bottom: -70rpx
  }

  25% , 75% {
    bottom: -4rpx
  }

  20% , 80% {
    bottom: 4rpx
  }
}

@keyframes rotate {
  0% {
    transform: rotate(-15deg)
  }

  25% , 75% {
    transform: rotate(0deg)
  }

  100% {
    transform: rotate(25deg)
  }
}
</style>
