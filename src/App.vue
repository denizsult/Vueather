<template>
  <div :class="darkMode ? 'appDark' : 'app'" id="app">
    <router-view @dark="darkMode = !darkMode" />
  </div>
</template>

<script>
import {themeConfig} from './EventBus'
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
  height: 100vh;
  width: 100vw;
  background-image: url("./assets/bg.png");
  background-size: cover;
  color:white !important;
}

.appDark {
    height: 100vh;
  width: 100vw;
  background-image: url("./assets/bg-dark.png") !important;
  background-size: cover;
  color:white !important;
}
</style>
