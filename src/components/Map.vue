<template>
  <v-container>
    <v-layout>
      <v-flex style="width: 500px; height: 500px;" id="mapid">
        <!-- A div containing the map element -->
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import leaflet from 'leaflet'
import axios from 'axios'
import {EventBus} from '../EventBus'

export default {
	data: () => ({
		map: null,
		marker: null,
	}),
  name: "Map",
	props: {},
	
  mounted() {
		  this.map = leaflet.map("mapid").setView([60.859515, 10.233114], 5);
      leaflet.tileLayer(
        "https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}",
        {
          maxZoom: 18,
          id: "mapbox.streets",
          accessToken:
            "pk.eyJ1IjoiYmFydGVrNzA4IiwiYSI6ImNqdHBrcGRyMzA1NjQ0ZHIwbHJsNnZvcm8ifQ.RjE2NlmMcunebCNLBiPlug"
        }
			).addTo(this.map);	
			this.map.on('click', e => this.setPin(e));
	},
	methods: {
		setPin: function(e){
			console.log(e.latlng)
			if(!this.marker){
			 this.marker = leaflet.marker(e.latlng).addTo(this.map)
			 EventBus.$emit("pin-click", e.latlng)
			}
			else{
				this.marker.setLatLng(e.latlng)
				EventBus.$emit("pin-click", e.latlng)
			}
		}
	}

};
</script>



