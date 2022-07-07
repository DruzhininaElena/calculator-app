<template>
  <div>
    <div
      class="bg"
    ></div>
  </div>
</template>

<script>

export default {
  name: 'BgApp'
}
</script>

<style lang="scss" scoped>
  .bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background-size: cover;
    background-repeat: no-repeat;
    transition: background-image 2s;
    background-size: cover;
    background-position: center;
    z-index: -1;
    background-color: #eed56d;
    &::after {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-repeat: repeat;
      animation: animate 480s cubic-bezier(0.28, 0.54, 0.47, 0.56) infinite normal;
      background-image: url("../assets/images/symbols.png");
      opacity: 0.2;
    }
  }
  @keyframes animate {
    0% {
      background-position: 0px 0px, 0px 0px, 0px 0px;
    }
    100% {
      background-position: 400px 1100px, 400px 400px, 600px 600px;
    }
  }
</style>
