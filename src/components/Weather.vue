<template>
  <v-container grid-list-md text-xs-center>

    <!--Vuetify Iterator, Iterate through data.list-->
    <v-data-iterator
      :items="data.list"
      :rows-per-page-items="rowsPerPageItems"
      :pagination.sync="pagination"
      content-tag="v-layout"
      row
      wrap
    >
      <template v-slot:item="props">
        <v-flex xs12 sm6 md4 lg3>
          <ForecastCard :item="props" :data="data"/>
        </v-flex>
      </template>
    </v-data-iterator>
  </v-container>
</template>

<script>
import ForecastCard from "./ForecastCard";

export default {
  name: "Weather",
  props: ["data", "city"],
  components: {
    ForecastCard
  },
  data() {
    return {

      //setup display data table pages
      rowsPerPageItems: [4, 8, 12],
      pagination: {
        rowsPerPage: 4
      }
    };
  },
  mounted() {}
};
</script>


