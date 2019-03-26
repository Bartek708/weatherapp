<template>
  <v-app>
    <v-toolbar >
    </v-toolbar>

    <v-content>
      <v-container fluid>
        <v-layout row wrap>
            <Map/>
            <Weather/>
        </v-layout>
      </v-container>
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
    Weather,
  },
  data () {
    return {
      //
    }
  }
}
</script>
