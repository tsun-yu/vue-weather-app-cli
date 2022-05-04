<template>
  <svg viewbox="-60 -30 100 100">
    <circle class="sun" cx="60" cy="40" r="22"></circle>
    <circle class="cloud" cx="52" cy="75" r="22"></circle>
    <circle class="cloud" cx="37" cy="75" r="22"></circle>
    <circle class="cloud" cx="22" cy="75" r="22"></circle>
    <circle class="cloud" cx="27" cy="55" r="15"></circle>
    <circle class="cloud" cx="45" cy="60" r="15"></circle>
  </svg>
</template>

<script>
export default {
  name: "CouldyIcon",
};
</script>

<style lang="scss">
svg {
  width: 100%;
  height: 100%;
  position: relative;
  .sun {
    fill: #ffd633;
    stroke: rgba(255, 214, 51, 0.5);
    stroke-width: 30px;
    stroke-dasharray: 5px;
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
  }
}

.cloud:before,
.cloud:after {
  content: "";
  background-color: #f3f3f3;
  display: block;
  width: 50px;
  height: 50px;
  border-radius: 100%;
  position: absolute;
}
.cloud:before {
  left: 16px;
  top: -28px;
}
.cloud:after {
  left: 35px;
  top: -20px;
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
svg .cloud {
}
</style>
