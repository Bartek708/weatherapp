<template>
  <v-app>
    <v-toolbar dark></v-toolbar>
    <Map/>
    <Weather :data="data"/>
  </v-app>
</template>

<script>
import Map from "./components/Map";
import Weather from "./components/Weather";
import axios from "axios";
import {EventBus} from './EventBus'

export default {
  name: "App",
  components: {
    Map,
    Weather
  },
  data() {
    return {
      data: []
    };
  },
  mounted() {
    //default oslo forecast
  },

  created(){
    EventBus.$on('pin-click', latlng => this.getWeather(latlng))    
  },

  methods: {
    getWeather(latlng) {
      const token = "3f51efbc33effda11899a0ace65c7aa3";
      axios
        .get(
          "http://api.openweathermap.org/data/2.5/forecast?lat=" +
            latlng.lat +
            "&lon=" +
            latlng.lng +
            "&units=metric&appid=" +
            token
        )
        .then(response => {
          this.data = response.data;
        });
    }
  }
};
</script>
