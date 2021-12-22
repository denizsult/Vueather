<template>
  <div :class="dark ? 'appDark' : 'app'" id="app">
    <router-view @dark="dark = !dark" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      dark: false,
    }
  },
}


</script>
<style>
@import "./assets/css/main.css";
.app {
  height: 100vh;
  width: 100vw;
  background-image: url("./assets/bg.png");
  background-size: cover;
  color:black !important
}

.appDark {
  height: 100vh;
  width: 100vw;
  background-image: url("./assets/bg-dark.png");
  background-size: cover;
  color:white !important;
 
}
</style>
