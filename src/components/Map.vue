<template>
    <v-container>
        <v-layout>
            <v-flex style="width: 500px; height: 500px;" id="mapid">
                <h1>Map</h1>
            </v-flex>
        </v-layout>
    </v-container>
</template>
<script>
import axios from 'axios'
import L from "leaflet"

export default {
    name: 'Map',
    cities: [],
    markers: null,
    data() {
        return {
            data: [],
            coords: [],
            cor: null,
            mymap: null,
            popup: null,
            marker: null,
            lat: null,
            lon: null,
        }
    },
    methods: {
        findIcon(icons) {
            let iconsArr = []
            icons.map((res) => (
                iconsArr.push(res.icon)
            ));
        },
        onClickButton(event) {
            //this.$emit('clicked', 'someValue')
        },
        test() {
            console.log("test")
        },
        setMarker(weatherIcon, lat, lon) {
            let weatherMark = L.icon({
                iconUrl: 'http://openweathermap.org/img/w/' + weatherIcon + '.png',
                iconSize: [80, 80]
            })

            let marker2 = L.marker([lat, lon], { icon: weatherMark }).addTo(this.mymap);
            //marker2.addTo(this.mymap);
            //console.log(marker2)
            this.markers.push(L.marker([lat, lon]))
        },
        setMarkers() {
            let iconsArr = []
            let long = []
            let lati = []
            let marker
            let markers = []

            this.markers = markers

            //console.log("CITIES")
            //console.log(this.cities.list)


            for (let i = 0; i < this.markers.length; i++) {
                this.mymap.removeLayer(this.markers[i])
            }
            //this.mymap.removeLayer(this.markers[0])

            for (let i = 0; i < this.cities.list.length; i++) {
                let weatherIcon = this.cities.list[i].weather[0].icon
                let lat = this.cities.list[i].coord.lat
                let lon = this.cities.list[i].coord.lon

                this.setMarker(weatherIcon, lat, lon)

            };

            //console.log(this.markers.length)

        },
        // on click sets a cursor to that location and removes previously declared curso
        onMapClick(e) {


            this.mymap.removeLayer(this.marker)
            this.marker = L.marker(e.latlng).addTo(this.mymap);
            this.popup
                .setLatLng(e.latlng)
                .setContent("You clicked the map at " + e.latlng.toString())
                .openOn(this.mymap);
            //console.log(e.latlng.toString())
            this.cor = e.latlng


            //console.log(this.cor.lat)
            this.fetchData()
        },
        fetchData() {
            this.lat = this.cor.lat
            this.lon = this.cor.lng

            this.$emit('clicked', this.cor)
            //console.log(this.lat)
            //fetches weather data
            var token = "&APPID=3f51efbc33effda11899a0ace65c7aa3"
            axios
                .get('http://api.openweathermap.org/data/2.5/find?lat=' + this.lat + '&lon=' + this.lon + '&cnt=5' + token)
                .then(response => (this.cities = response.data,

                    this.setMarkers()

                ))

            //console.log(this.cities)

            /*this.data.map((res) => (
                console.log(res)
            ));*/
        },
        setMap() {

            L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
                maxZoom: 18,
                id: 'mapbox.streets',
                accessToken: 'pk.eyJ1IjoiYmFydGVrNzA4IiwiYSI6ImNqdHBrcGRyMzA1NjQ0ZHIwbHJsNnZvcm8ifQ.RjE2NlmMcunebCNLBiPlug'
            }).addTo(this.mymap);

            this.popup = L.popup();
            let icon;

            /*let icons = [{
                    icon: "01d",
                    lat: 60.84,
                    lon: 10.0618
                }, {
                    icon: "02d",
                    lat: 60.6987,
                    lon: 10.3519
                },
                {
                    icon: "01d",
                    lat: 60.7957,
                    lon: 10.6916
                },
                {
                    icon: "01d",
                    lat: 60.9333,
                    lon: 10.7
                },
                {
                    icon: "01d",
                    lat: 60.6833,
                    lon: 10.6167
                },
                {
                    icon: "02d",
                    lat: 60.7947,
                    lon: 10.6929
                },
                {
                    icon: "02d",
                    lat: 61.1146,
                    lon: 10.4674
                },
                {
                    icon: "01d",
                    lat: 60.8878,
                    lon: 9.6414
                }
            ]
            */

            /*
            let iconsArr = []
            let long = []
            let lati = []
            icons.map((res) => (
                iconsArr.push(res.icon) && long.push(res.lon) && lati.push(res.lat)
            ));


            let weatherIcon = "01d"
            let weatherMark;

            for (let i = 0; i < iconsArr.length; i++) {
                let weatherIcon = iconsArr[i];
                let weatherMark = L.icon({
                    iconUrl: 'http://openweathermap.org/img/w/' + weatherIcon + '.png',
                    iconSize: [80, 80]
                })

                let marker2 = L.marker([String(lati[i]), String(long[i])], { icon: weatherMark }).addTo(this.mymap);
            }
            */

            //sets current location to Oslo
            // L.marker([51.941196,4.512291], ).addTo(mymap);
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