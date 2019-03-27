<template>
    <v-app>
        <v-toolbar dark></v-toolbar>
        <Map @clicked="onClickChild" />
        <Weather :data="data" />
        
    </v-app>
</template>
<script>
import Map from "./components/Map";
import Weather from "./components/Weather";
import axios from "axios";





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
        this.getWeather(57.574779, 10.744629);

        //Map.onMapClick

    },

    methods: {
        test() {
            Map.methods.onMapClick()
        },
        onClickChild(value) {
            this.getWeather(value.lat, value.lng)
            //console.log(value.lat) // someValue
        },
        getWeather(lat, lon) {
            const token = "3f51efbc33effda11899a0ace65c7aa3";
            axios
                .get(
                    "http://api.openweathermap.org/data/2.5/forecast?lat=" +
                    lat +
                    "&lon=" +
                    lon +
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