<template>

	<v-flex xs6 order-lg2>
		<h1>Map</h1>

		<!-- A div containing the map element -->
    	<div  style="width: 500px;
 	 height: 500px;" id="mapid" >
		</div>
	</v-flex>

</template>


<script>
export default {
  name:'Map',
  props: {
    msg: String
	},
	
	  mounted() {
  	//creates the default map upon closing the application, here being a map over norway with zoom 5
  	var mymap = L.map('mapid').setView([60.859515, 10.233114], 5);
  	L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
    maxZoom: 18,
    id: 'mapbox.streets',
    accessToken: 'pk.eyJ1IjoiYmFydGVrNzA4IiwiYSI6ImNqdHBrcGRyMzA1NjQ0ZHIwbHJsNnZvcm8ifQ.RjE2NlmMcunebCNLBiPlug'
	}).addTo(mymap);
	var popup = L.popup();
	//sets current location to Oslo
	var marker = L.marker([60.859515, 10.233114]).addTo(mymap);
	// on click sets a cursor to that location and removes previously declared curso
	function onMapClick(e) {
		mymap.removeLayer(marker)
		marker = L.marker(e.latlng).addTo(mymap);
    	popup
        .setLatLng(e.latlng)
        .setContent("You clicked the map at " + e.latlng.toString())
        .openOn(mymap);  
	}
	mymap.on('click', onMapClick);
  },

}
</script>



