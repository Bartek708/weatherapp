<template>
    
    
  
    <v-flex>
      

    <h1>Weather</h1>
    {{data}}

  </v-flex>
 

  
</template>

<script>
import axios from 'axios'

export default {
  props: {
    msg: String
  },
  data () {
    	return {
    		data: []
    	}
  	},
  mounted () {
    var token = "&APPID=3f51efbc33effda11899a0ace65c7aa3"
      axios
        .get('http://api.openweathermap.org/data/2.5/find?lat=55.5&lon=37.5&cnt=10&APPID=3f51efbc33effda11899a0ace65c7aa3')
        .then(response => (this.data = response.data))
    }
}
</script>


