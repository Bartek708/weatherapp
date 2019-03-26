<template>
  <v-app>
    <v-content>
      <Map/>
      <Weather/>
    </v-content>
  </v-app>
</template>

<script>
import Map from './components/Map'
import Weather from './components/Weather'

export default {
  name: 'App',
  components: {
    Map,
    Weather
  },
  data () {
    return {
      //
    }
  }
}
</script>
