<template>
  <div id="app">
    <span class="text">123</span>
    <router-view />
  </div>
</template>

<script>
export default {};
document.addEventListener("DOMContentLoaded", () => {
  const html = document.querySelector("html");
  let fontSize = window.innerWidth / 10;
  fontSize = fontSize > 50 ? 50 : fontSize;
  html.style.fontSize = fontSize + "px";
});
</script>

<style lang="scss" scoped>
@import "./assets/styles/global";
.text {
  font-family: "Days One";
  font-size: px2rem(20);
}
</style>
