<template>
    <v-container>
        <v-layout>
            <v-flex style="width: 500px; height: 500px;" id="mapid">
            </v-flex>
        </v-layout>
        <v-select label="City" item-text="city,lat,lon" :items="options" @input="setCityOption"></v-select>
        <p onmouseover="this.style.color='blue'" style="cursor: pointer;" onmouseout="this.style.color='black'" v-on:click="setCity(60.392984, 5.336146)">Bergen</p>
        <p onmouseover="this.style.color='blue'" style="cursor: pointer;" onmouseout="this.style.color='black'" v-on:click="setCity(63.418366, 10.448214)">Trondheim</p>
        <p onmouseover="this.style.color='blue'" style="cursor: pointer;" onmouseout="this.style.color='black'" v-on:click="setCity(69.654148, 18.967907)">Tromsø</p>
        <p onmouseover="this.style.color='blue'" style="cursor: pointer;" onmouseout="this.style.color='black'" v-on:click="setCity(59.908804, 10.748939)">Oslo</p>
    </v-container>
</template>
<script>
import axios from 'axios'
import L from "leaflet"
import App from "../App";

export default {
    name: 'Map',
    cities: [],
    markers: null,
    data() {
        return {
            data: [],
            coords: [],
            cor: null,
            selopt: null,
            mymap: null,
            popup: null,
            marker: null,
            lat: null,
            lon: null,
            options: [
                { city: "Bergen", lat: 60.392984, lon: 5.336146 },
                { city: "Trondheim", lat: 63.418366, lon: 10.448214 },
                { city: "Tromsø", lat: 69.654148, lon: 18.967907 },
                { city: "Oslo", lat: 59.908804, lon: 10.748939 }
            ]
        }
    },
    methods: {
        findIcon(icons) {
            let iconsArr = []
            icons.map((res) => (
                iconsArr.push(res.icon)
            ));
        },
        test() {
            console.log("test")
        },
        // sets marker with weather icon on this location
        setMarker(weatherIcon, lat, lon) {
            let weatherMark = L.icon({
                iconUrl: 'http://openweathermap.org/img/w/' + weatherIcon + '.png',
                iconSize: [80, 80]
            })

            let marker2 = L.marker([lat, lon], { icon: weatherMark }).addTo(this.mymap);
            this.markers.push(L.marker([lat, lon]))
        },
        // adds marker to mymap
        setMarkerSimple(lat, lon) {
            let marker2 = L.marker([lat, lon]).addTo(this.mymap);
        },
        // jumps and zooms to the city when clicked on map
        setCity(lat, lon) {
            this.mymap.flyTo([lat, lon], 8)
        },
        // jumps and zooms to the city from the option menu
        setCityOption(city) {

            this.mymap.flyTo([city.lat, city.lon], 8)
            //this.setMarkerSimple(city.lat, city.lon).addTo(this.mymap);

        },
        // finds 5 nearest cities/zones by the marker and set weather marks that 
        // corresponds to current weather info for that zone
        setMarkers() {
            let iconsArr = []
            let long = []
            let lati = []
            let marker
            let markers = []

            this.markers = markers

            for (let i = 0; i < this.markers.length; i++) {
                this.mymap.removeLayer(this.markers[i])
            }

            for (let i = 0; i < this.cities.list.length; i++) {
                let weatherIcon = this.cities.list[i].weather[0].icon
                let lat = this.cities.list[i].coord.lat
                let lon = this.cities.list[i].coord.lon

                this.setMarker(weatherIcon, lat, lon)

            };
        },
        // on click sets a cursor to that location and removes previously declared curso
        onMapClick(e) {


            this.mymap.removeLayer(this.marker)
            this.marker = L.marker(e.latlng).addTo(this.mymap);
            this.cor = e.latlng
            this.fetchData()
        },
        // finds 5 nearest cities/zones by the marker 
        fetchData() {
            this.lat = this.cor.lat
            this.lon = this.cor.lng

            //sends coordinates to parent compenent <App>
            this.$emit('clicked', this.cor)

            var token = "&APPID=3f51efbc33effda11899a0ace65c7aa3"
            axios
                .get('http://api.openweathermap.org/data/2.5/find?lat=' + this.lat + '&lon=' + this.lon + '&cnt=5' + token)
                .then(response => (this.cities = response.data,
                    this.setMarkers()
                ))

        },
        setMap() {
            // fetches mapobject from leaflet and add to mymap
            L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
                maxZoom: 18,
                id: 'mapbox.streets',
                accessToken: 'pk.eyJ1IjoiYmFydGVrNzA4IiwiYSI6ImNqdHBrcGRyMzA1NjQ0ZHIwbHJsNnZvcm8ifQ.RjE2NlmMcunebCNLBiPlug'
            }).addTo(this.mymap);

            this.popup = L.popup();
            let icon;

            //sets default location to oslo
            this.lat = 60.859515;
            this.lon = 10.233114;
            this.marker = L.marker([this.lat, this.lon]).addTo(this.mymap);
            this.mymap.on('click', this.onMapClick);
        }
    },
    mounted() {
        //creates the default map upon closing the application, here being a map over norway with zoom 5
        this.mymap = L.map('mapid').setView([60.859515, 10.233114], 5);
        this.setMap()
    }
}
</script>