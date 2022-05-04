<script setup></script>

<template>
  <svg viewbox="-60 -30 100 100">
    <line class="rain" x1="39" y1="30" x2="39" y2="50"></line>
    <line class="rain" x1="50" y1="20" x2="50" y2="40"></line>
    <line class="rain" x1="61" y1="35" x2="61" y2="60"></line>
    <circle class="cloud" cx="64" cy="45" r="22"></circle>
    <circle class="cloud" cx="49" cy="45" r="22"></circle>
    <circle class="cloud" cx="34" cy="45" r="22"></circle>
    <circle class="cloud" cx="39" cy="25" r="15"></circle>
    <circle class="cloud" cx="57" cy="30" r="15"></circle>
  </svg>
</template>

<style lang="scss" scoped>
svg {
  width: 100%;
  height: 100%;
  position: relative;

  .rain {
    width: 2px;
    height: 10px;
    position: absolute;
    left: 45px;
    bottom: 15px;
    animation: raindrop 2s infinite linear;
    background-color: #f3f3f3;
    stroke: #4dacff;
  }

  .cloud {
    width: 100px;
    height: 36px;
    position: absolute;
    left: 200px;
    bottom: 50px;
    border-radius: 20px;
    animation: clouddrift 4s infinite;
    background-color: #f3f3f3;
    box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.3);
    fill: #ccc;
    transition: 0.5s;

    &::before,
    &::after {
      content: "";
      background-color: #f3f3f3;
      display: block;
      width: 50px;
      height: 50px;
      border-radius: 100%;
      position: absolute;
    }

    &::before {
      left: 16px;
      top: -28px;
    }

    &::after {
      left: 35px;
      top: -20px;
    }
  }
}

@keyframes raindrop {
  0% {
    transform: translateY(0px);
  }
  100% {
    transform: translateY(50px);
    opacity: 0;
  }
}

@keyframes clouddrift {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-10px);
  }
  100% {
    transform: translateY(0px);
  }
}
</style>
