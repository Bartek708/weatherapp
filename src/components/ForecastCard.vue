<template>

  <v-card>
    <v-card-title>
      <h4>{{ item.item.dt_txt }}, {{data.city.name}}, {{data.city.country}}</h4>
    </v-card-title>
    <v-divider></v-divider>
    <v-list dense>
      <v-list-tile>
        <v-list-tile-content>Temperature:</v-list-tile-content>
        <v-list-tile-content class="align-end">{{ item.item.main.temp }} ºC</v-list-tile-content>
      </v-list-tile>
      <v-list-tile>
        <v-list-tile-content>Humidity:</v-list-tile-content>
        <v-list-tile-content class="align-end">{{ item.item.main.humidity }}%</v-list-tile-content>
      </v-list-tile>
      <v-list-tile>
        <v-list-tile-content>Weather</v-list-tile-content>
        <v-list-tile-content class="align-end">{{ item.item.weather[0].description}}</v-list-tile-content>
      </v-list-tile>
      <v-img :src="img"  height="75px" width="75px"></v-img>
    </v-list>
  </v-card>
</template>

<script>
export default {
    name: 'ForecastCard',
    props: ["item", "data", "city"],

    data: () => ({
        img: ""
    }),

    created(){
        this.img = this.getIcon()
    },

    updated(){
        this.img = this.getIcon()
    },

    methods:{
        getIcon(){
            return `http://openweathermap.org/img/w/${this.item.item.weather[0].icon}.png` 
        }
    }
}
</script>