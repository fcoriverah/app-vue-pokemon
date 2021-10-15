<template>
  <div class="loading-container">
    <img src="../assets/Loader.gif" alt="" />
  </div>
</template>

<script>
export default {
  name: "Loading",
  props: ["isLoading"],
};
</script>


<style lang="less">
body {
  background-color: #e5e5e5;
}
.loading-container {
  height: 100%;
  width: 100%;
  position: absolute;
  z-index: 10000;
  background-color: #e5e5e5;
  img {
    margin: auto;
    position: fixed;
    top: 50%;
    left: 53%;
    transform: translate(-50%, -50%);
  }
}

// ----------------- MOBILE -----------------

@media (max-width: 615px) {
  .loading-container {
    width: 98%;
    height: 98%;
    img {
      margin: auto;
      position: fixed;
      top: 50%;
      left: 52%;
      transform: translate(-50%, -50%);
    }
  }
}
</style>