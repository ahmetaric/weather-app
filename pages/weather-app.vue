<template>
  <v-container>
    <!-- <v-layout> -->
    <h1 class="text-h4 text-center ma-4">Weather App</h1>
    <v-flex xs-12>
      <v-card color="blue ligthen-2" dark>
        <v-card-text>
          <v-layout  class="justify-center">
            <v-flex v-if="weather.weather" class="text-center" >
            <h4 class="mb-3">Temperature</h4>
            <h1 class="text-h4">{{ weather.name }}</h1>
            <img :src="icon" alt="weather-icon">
            <p class="text-h4">
              <span>{{Math.round(weather.main.temp)}}°C</span>
              <span class="caption ml-4">{{ weather.weather[0].description }}</span>
            </p>
          </v-flex>
          <v-flex v-if="weather.weather" class="text-center" >
            <h4 class="mb-3">Wind & Pressure</h4>
            <h3 class="headline mt-4">Wind : {{ weather.wind.speed }} m/s ({{ weather.wind.deg }})°</h3>
            <h3 class="headline mt-5">Humidity : {{ weather.main.humidity }} %</h3>
            <h3 class="headline mt-5">Pressure : {{ weather.main.pressure }} hPa</h3>
          </v-flex>
          <v-flex v-if="weather.weather" class="text-center" >
            <h4 class="mb-3">Other</h4>
             <h3 class="headline mt-4">Max Temperature : {{Math.round(weather.main.temp_max )}}°C</h3>
             <h3 class="headline mt-4">Min Temperature : {{Math.round(weather.main.temp_min )}}°C</h3>
          </v-flex>
          </v-layout>
        </v-card-text>
      </v-card>
    </v-flex>
    <v-flex xs-12 class="mt-4">
     <v-form @submit.prevent="getWeather">
       <v-text-field
        v-model="city"
        label="Enter city name"
        solo 
        required
        append-icon="mdi-map-marker"
        @click:append="getLocation"
      ></v-text-field>
     </v-form>
    </v-flex>
    <!-- </v-layout> -->
  </v-container>
</template>

<script>

export default {
  
  data() {
    return {
      city:'',
      weather:[]
    }
  },
  computed:{
    icon(){
      return this.weather.weather
      ? `https://openweathermap.org/img/wn/${
                 this.weather.weather[0].icon}@2x.png`
      : ""           
    },
  },
 
  methods: {
    getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.getWeatherLoc)
      } else {
        alert('Geolocation is not supported by this browser.')
      }
    },
    getWeatherLoc(position){
      const lat = position.coords.latitude
      const lon = position.coords.longitude
      this.$axios
      .$get(
        `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=ab4ad2180412fe2746ca961bd35c26df`
      )
      .then((response) => {
        this.weather = response
      })
    },
    getWeather(){
      this.$axios
      .$get(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ab4ad2180412fe2746ca961bd35c26df`
      )
      .then((response) => {
        this.weather = response
        this.city = ""
      })
    },
}
}
</script>

<style></style>
