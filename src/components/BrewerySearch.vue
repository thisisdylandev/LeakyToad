<template>
  <div class="brewery-search-container">
    <div class="brewery-search">
      <b-form-input v-model="city" placeholder="City" class="mt-3"></b-form-input>
      <b-form-select v-model="state" :options="statesList" size="sm" class="mt-3">
        <template v-slot:first>
          <option :value="null" disabled>-- state --</option>
        </template>
      </b-form-select>    
      <b-form-select v-model="selectedType" :options="breweryTypes" size="sm" class="mt-3">
        <template v-slot:first>
          <option :value="null" disabled>-- type --</option>
        </template>
      </b-form-select>
      <b-button v-on:click="findBeer" class="btn-find">find breweries!</b-button>
      </div>
    <div>
      <div class="spinner-border" role="status" v-if="loading">
        <span class="sr-only">loading...</span>
      </div>
      <div v-else class="brewery-grid">
        <div v-for="(brewery, index) in breweries" v-bind:key="index">
          <Brewery :brewery="brewery"/>
        </div>
      </div>  
    </div>
  </div>
</template>

<script>
import Brewery from './Brewery';

import axios from 'axios';
import states from '../assets/states.json';
import types from '../assets/types.json';

export default {
  name: 'BrewerySearch',
  components: {
    Brewery,
  },
  data () {
    return {
      breweryTypes: types,
      selectedType: null,
      city: null,
      state: null,
      statesList: states,
      requestUrl: 'https://api.openbrewerydb.org/breweries?',
      breweries: {},
      toggle: false,
      loading: false
    }
  },
  methods: {
    findBeer: function () {
      /* eslint-disable no-console */
      let options = '';
      if (this.city != null && this.city != '') {
        options += 'by_city=' + this.city
      }

      if (this.state != null) {
        options === '' ? options += 'by_state=' + this.state : options += '&by_state=' + this.state
      }

      if (this.selectedType != null) {
        options === '' ? options += 'by_type=' + this.selectedType : options += '&by_type=' + this.selectedType
      }
      
      this.loading = true;
      axios
        .get(this.requestUrl + options)
        .then(response => {
          this.breweries = response.data;
          this.loading = false;
        })
    }
  }
}
</script>

<style scoped>
.brewery-search-container{
  max-width: 60%;
  margin: 0 auto;
}

.brewery-search{
  max-width: 90%;
  margin: 0 auto;
}

.brewery-grid{
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.btn-find{
  margin: 2% 0;
}
</style>
