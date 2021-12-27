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
  min-height: 100vh;
  height: 100%;
  background-image: url("./assets/bg.png") !important;
  background-size: cover;
  width: 100%;


  color: white !important;
}

.appDark {
  width: 100%;
  min-height: 100vh;
  height: 100%;
  background-image: url("./assets/bg-dark.png") !important;
  background-size: cover;
  color: white !important;
}
</style>
