<template>
  <div class="containter">
    
    
    <div class="switch">
      <input
        @input="darkMode = !darkMode, modeChange()"
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



    <Search @showCards="showCard = true"   @daily="getDaily"  @seven="getSeven"    />

    <Cards :daily="dailyData" :seven="sevenData"  v-if="showCard" /> 

    
  </div>
</template>

<script>
import Cards from '../components/cards.vue'
import Search from '../components/search.vue'
import {themeConfig} from '../EventBus'


export default {
    name: "Home",
    components: { Cards, Search },
    data() {
        return {
            sevenData: [],
            dailyData:[],
            darkMode: false,
            showCard: true,
            
        };
    },
    methods: {
      modeChange() {
        themeConfig.$emit('dark', this.darkMode)
      },
      getDaily(data){
        this.dailyData = data
      },

       getSeven(data){
        this.sevenData = data
      }
       
    },
    
}
</script>

 