<template>
  <div class="results">
    <div :style="[darkMode ? $store.state.dark : { color: 'black' }]" class="card-1">
      <h4 style="display:inline">Current Weather</h4>

      <div class="cards">
        <div class="today">
          <h2>{{ daily.name }}</h2>

          <div class="daily">
            <img
              :src="require(`../assets/weater_elements/${weathers[daily.weather[0].main]}.svg`)"
              width="220px"
              alt
            />

            <p>{{ parseInt(daily.main.temp) }}°</p>
          </div>

          <h2>{{ daily.weather[0].main }}</h2>
        </div>

        <div class="todayDetails">
          <h3>Feels Like {{ parseInt(daily.main.feels_like) }}°</h3>
          <h3>
            <img width="64" src="../assets/weater_elements/humity.png" alt />
            {{ daily.main.humidity }}%
          </h3>
          <h3>
            <img src="../assets/weater_elements/down.png" alt />
            {{ parseInt(daily.main.temp_min) }}°
          </h3>

          <h3>
            <img src="../assets/weater_elements/up.png" alt />
            {{ parseInt(daily.main.temp_max) }}°
          </h3>
          <h3>
            <img src="../assets/weater_elements/wind.png" alt />
            {{ daily.wind.speed }} km/h
          </h3>
        </div>
      </div>
    </div>

    <div :style="[darkMode ? $store.state.dark : { color: 'black' }]" class="card-2">
      <div class="days">
        <p v-for="i in daysIndex" :key="i">
          {{
            days[i].slice(0, 3)
          }}
        </p>
      </div>

      <div class="daysDetail">
        <span style="display:flex; flex-direction:column; align-items:center " v-for="data in seven.list" :key="data">
          <img
            :src="require(`../assets/weater_elements/${weathers[data.weather[0].main]}.svg`)"
            width="100px"
            alt
          />

          <p>
            {{ parseInt(data.temp.day) }}° / {{ parseInt(data.temp.night) }}°
          </p>
        </span>
      </div>
    </div>
  </div>
</template>





<script>
import { themeConfig } from '../EventBus'
export default {
  name: 'Cards',
  props: ['daily', 'seven'],
  data() {
    return {

      darkMode: false,
      showCard: false,
      weathers: {
        Snow: "snowy",
        Clouds: "cloudy",
        Rain: "rainy",
        Clear: "sunny",
        Thunderstorm: "thunder",
      },
      days: ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
      daysIndex: [],




    };
  },
  mounted() {
    themeConfig.$on('dark', (data) => {
      this.darkMode = data
    })

    var currentDate = new Date();
    var nextWeek = new Date(currentDate.getTime() + 7 * 24 * 60 * 60 * 1000);
    var days = []
    while (currentDate <= nextWeek) {
      days.push(new Date(currentDate).getDay());
      currentDate.setDate(currentDate.getDate() + 1);
    }
    this.daysIndex = days.slice(1);






  },





}
</script>


<style>
.results {
  display: flex;
  flex-direction: column;
  width: 60vw !important;
  height: 100%;
}

.cards {
  display: flex;
  width: 100%;
  margin-top: 20px;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
}
.card-1 {
  width: 100%;
  background-color: white;
  border-radius: 10px;
  margin-top: 20px;
  padding: 20px;
}

.today {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.todayDetails {
  display: flex;
  margin-top: 30px;
  flex-direction: column;
  align-items: flex-start;
}
.daily {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  font-size: 90px;
}
.card-2 {
  width: 100%;
  height: 30%;
  background-color: white;
  border-radius: 10px;
  margin-top: 20px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: stretch;
  overflow: auto;
}
.days {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  font-size: 23px;
}
.daysDetail {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
    font-size: 23px;

}
</style>
 