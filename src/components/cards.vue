<template>
  <div class="results">
    <div :style="[darkMode ? $store.state.dark : { color: 'black' }]" class="card-1">
      <h3 style="display:inline">Current Weather</h3>

      <div class="cards">
        <div class="bigCard">
          <h2>{{daily.name}}</h2>
          <img src="../assets/weater_elements/cloud_sun.png" width="200px" alt />
          <h2>{{daily.weather[0].description}}</h2>
        </div>

       
      </div>
    </div>

    <div :style="[darkMode ? $store.state.dark : { color: 'black' }]" class="card-2"></div>
  </div>
</template>





<script>
import { themeConfig } from '../EventBus'
export default {
  name: 'Cards',
  props:['daily', 'seven' ],
  data() {
    return {
      sevenDay: [],
      darkMode: false,
      showCard: false,
    };
  },
  mounted() {
    themeConfig.$on('dark', (data) => {
      this.darkMode = data
    })
  },


}
</script>


<style>
.results {
  display: flex;
  flex-direction: column;
  width: 60% !important;
  height: 100%;
}

.cards {
  display: flex;
  width: 100%;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
}
.card-1 {
  width: 100%;
  height: 60%;
  background-color: white;
  border-radius: 10px;
  margin-top: 20px;
  padding: 20px;
}

.bigCard{
  display: flex;
  flex-direction: column;
  align-items: center;
}

.card-2 {
  width: 100%;
  height: 30%;
  background-color: white;
  border-radius: 10px;
  margin-top: 20px;
  padding: 20px;
}
</style>
 