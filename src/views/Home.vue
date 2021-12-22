<template>
  <div class="containter">
    <div class="switch">
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

    <div class="header">
      <h1>Vueather</h1>
    </div>

    <b-input-group class="input">
      <button class="inputpre" :style="[darkMode ? styles.dark : '']">
        <img src="../assets/search.png" alt />
      </button>

      <v-select
        :class="darkMode ? 'vselect dark' : 'vselect'"
        placeholder="Search for location"
        style="border:none"
        v-model="selectedCity"
        
        @search="onSearch"
        :options="cities"
        label="name" 
        :filterable="false"  
      >
        <template slot="no-options">type to search for weather..</template>
        <template slot="option" slot-scope="option">
          <div class="d-center">{{ option.administrative[0] }}, {{ option.country }}</div>
        </template>
      </v-select>

      <button :style="[darkMode ? styles.dark : '']" @click="getLocation" class="inputpre">
        <img src="../assets/target.png" alt />
      </button>
    </b-input-group>

    <div v-if="showCard" class="results">
      <div :style="[darkMode ? styles.dark : { color: 'black' }]" class="card-1">
        <h2>Current Weather</h2>

        <div class="cards">
          <div class="bigCart">{{ sevenDay }}</div>
          <div class="detailCard">dsasad</div>
        </div>
      </div>

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
      selectedCity: '',
      cities: [],



      lat: '',
      lon: '',

      styles: {
        dark: { backgroundColor: '#72ABDC', border: 'none', color: 'white' }
      }
    }
  },
  methods: {
    getSevenDay(data) {
      axios(`https://api.openweathermap.org/data/2.5/forecast/daily?lat=+${data.coords.latitude}&lon=${data.coords.longitude}&appid=20571ab45c74dc2a1897b60c5b8047a1`)
        .then(response => {
          this.sevenDay = response.data;

        })
    },


    onSearch(search, loading) {
      if (search.length) {
        loading(true);
        this.search(loading, search, this);
      }
    },


    search(loading, search) {

      axios('https://places-dsn.algolia.net/1/places/query', {
        params: {
          query: search,
          language: 'en',
          type: 'city'

        }
      }).then(response => {
        loading(false);
        this.cities = response.data.hits.slice(0, 5);
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

.cards {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  padding-top: 30px;
}

.vselect .vs__search::placeholder,
.vselect .vs__dropdown-toggle {
  background: white;
  border: none;
  border-radius: 0;
  width: 100%;
  height: 100%;
  color: black;
}

.dark .vs__search::placeholder,
.dark .vs__dropdown-toggle
{
  background: #72abdc;
  color: white;
  font-weight: 500;
}
</style>
 