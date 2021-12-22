<template>
  <div class="containter">
    <div class="header">
      <h1>VUE Weather App</h1>

      <div class="header-right">
        <input
          @change="$emit('dark')"
          v-model="darkMode"
          type="checkbox"
          id="toggle"
          class="toggle--checkbox"
        />
        <label for="toggle" class="toggle--label"></label>
        <div class="background"></div>
      </div>
    </div>

    <b-input-group class="input">
      <button class="inputpre" :style="[darkMode ? styles.dark : '']">
        <img src="../assets/search.png" alt />
      </button>

      <input
        type="text"
        placeholder="Search for location"
        style="border:none"
        :style="[darkMode ? styles.dark : '']"
      />
      <button :style="[darkMode ? styles.dark : '']" @click="getLocation" class="inputpre">
        <img src="../assets/target.png" alt />
      </button>
    </b-input-group>

    <div v-if="showCard" class="results">
      <div :style="[darkMode ? styles.dark : '']" class="card-1"></div>

      <div :style="[darkMode ? styles.dark : '']" class="card-2">dsadsa</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Home',

  data() {
    return {
      darkMode: false,
      showCard: false,
      sevenDay: [],
      lat: '',
      lon: '',

      styles: {
        dark: { backgroundColor: '#235479', border: 'none', color: '#4085b9' }
      }
    }
  },
  methods: {
    getSevenDay(data) {
      axios(`https://api.openweathermap.org/data/2.5/forecast/daily?lat=+${data.coords.latitude}&lon=${data.coords.longitude}&appid=20571ab45c74dc2a1897b60c5b8047a1`)
        .then(response => {
          this.sevenDay = response.body.list;
          console.log(this.sevenDay);
        })
    },

    getLocation() {

      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(data => {
          this.getSevenDay(data)
          this.showCard = true;
        })
      } else {
        console.log("Geolocation is not supported by this browser.");
      }
    },
  },




}
</script>


<style>
.results {
  display: flex;
  flex-direction: column;
  width: 60% !important;
  height: 80%;

  margin-top: 20px;
}

.card-1 {
  width: 100%;
  height: 60%;
  background-color: white;
  border-radius: 10px;
  margin-top: 20px;
  padding: 20px;
}

.card-2 {
  width: 100%;
  height: 40%;
  background-color: white;
  border-radius: 10px;
  margin-top: 20px;
  padding: 20px;
}
</style>
 