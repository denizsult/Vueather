<template>
    <div class="input">
        <b-input-group class="input">
            <button class="inputpre" :style="[darkMode ? $store.state.dark : '']">
                <img src="../assets/search.png" alt />
            </button>

            <v-select
                :class="darkMode ? 'vselect dark' : 'vselect'"
                placeholder="Search for location"
                style="border:none"
                v-model="selectedCity"
                @input="weatherLocation()"
                @search="onSearch"
                :options="cities"
                label="name"
                :filterable="false"
            >
                <template slot="no-options">type to search for weather..</template>
                <template slot="option" slot-scope="option">
                    <div class="d-center">{{ option.name }}</div>
                </template>
                <template slot="selected-option" slot-scope="option">
                    <div class="selected d-center">
                        <div class="d-center">{{ option.name }}</div>
                    </div>
                </template>
            </v-select>

            <button
                :style="[darkMode ? $store.state.dark : '']"
                @click="getLocation"
                class="inputpre"
            >
                <img src="../assets/target.png" alt />
            </button>
        </b-input-group>
    </div>
</template>
<script>
import axios from 'axios'
import { themeConfig } from '../EventBus'

export default {

    data() {
        return {
            darkMode: false,
            selectedCity: "",
            cities: [],
            lat: "",
            lon: "",

        }

    },

    mounted() {
        themeConfig.$on('dark', (data) => {
            this.darkMode = data
        })

    },


    methods: {

        // City Search

        onSearch(search, loading) {
            this.cities = [];
            if (search.length) {
                loading(true);
                this.search(loading, search, this);
            }
        },
        search(loading, search) {
            axios("https://places-dsn.algolia.net/1/places/query", {
                params: {
                    query: search,
                    language: "en",
                    type: "city"
                }
            }).then(response => {
                loading(false);
                let cities = new Set();
                response.data.hits.forEach(hit => {
                    cities.add(hit.administrative[0]);
                });
                this.cities = Array.from(cities);
            });
        },





        //With Select

        async weatherLocation() {
            let city = this.selectedCity;


            await axios(`https://api.openweathermap.org/data/2.5/forecast/daily?q=${city}&units=metric&appid=20571ab45c74dc2a1897b60c5b8047a1`).then((res) => {
                this.$emit('seven', res.data)

            })
            await axios(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=20571ab45c74dc2a1897b60c5b8047a1`).then((res) => {
                this.$emit('daily', res.data)
            })
            this.$emit('showCards');


        },

        //With Geolocation

        async getGeolocation(data) {
            await axios(`https://api.openweathermap.org/data/2.5/forecast/daily?lat=${data.coords.latitude}&lon=${data.coords.longitude}&units=metric&appid=20571ab45c74dc2a1897b60c5b8047a1`).then((res) => {
                this.$emit('seven', res.data)
            });

            await axios(`https://api.openweathermap.org/data/2.5/weather?lat=${data.coords.latitude}&lon=${data.coords.longitude}&units=metric&appid=20571ab45c74dc2a1897b60c5b8047a1`)
                .then((res) => {
                    this.$emit('daily', res.data)
                })

            this.$emit('showCards');

        },


        // Get User Location 
        getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(data => {
                    this.getGeolocation(data);
                    this.$emit('showCards');
                });
            }
            else {
                console.log("Geolocation is not supported by this browser.");
            }
        },
    }
}
</script>


<style>
.vselect .vs__search::placeholder,
.vselect .vs__dropdown-toggle {
    background: white;
    border: none;
    border-radius: 0;
    width: 100%;
    height: 50px;
    color: black;
}

.dark .vs__search::placeholder,
.dark .vs__dropdown-toggle,
.dark .vs__dropdown-menu {
    background: #72abdc;
    color: white !important;
    font-weight: 500;
}
</style>