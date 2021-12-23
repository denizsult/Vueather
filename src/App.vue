<template>
  <div :class="darkMode ? 'appDark' : 'app'" id="app">
    <router-view @dark="darkMode = !darkMode" />
  </div>
</template>

<script>
import { themeConfig } from './EventBus'
export default {
  data() {
    return {
      darkMode: false,
    }
  },

  mounted() {
    themeConfig.$on('dark', (data) => {
      this.darkMode = data
    })
  },
}


</script>
<style>
@import "./assets/css/main.css";
.app {
  height: 120vh;

  width: 100%;
  background-image: url("./assets/bg.png");
  background-size: cover;
  color: white !important;
}

.appDark {
  width: 100%;
  height: 120vh;

  background-image: url("./assets/bg-dark.png") !important;
  background-size: cover;
  color: white !important;
}


</style>
