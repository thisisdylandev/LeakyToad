<template>
  <div class="brewery-search">
    <b-form-input v-model="city" placeholder="City" class="mt-3"></b-form-input>
    <b-form-select v-model="state" :options="statesList" size="sm" class="mt-3">
      <template v-slot:first>
        <option :value="null" disabled>-- State --</option>
      </template>
    </b-form-select>    
    <b-form-select v-model="selectedType" :options="breweryTypes" size="sm" class="mt-3">
      <template v-slot:first>
        <option :value="null" disabled>-- Type --</option>
      </template>
    </b-form-select>
    <b-form-group label="Options:">
      <b-form-checkbox-group
        id="checkbox-group-1"
        v-model="selectedTags"
        :options="breweryTags"
        name="brewery-tags"
      ></b-form-checkbox-group>
    </b-form-group>
    <b-button v-on:click="findBeer">Find Breweries!</b-button>
    <div v-if="toggle">
      <div v-for="(brewery, index) in breweries" v-bind:key="index">
        <Brewery :brewery="brewery"/>
      </div>
    </div>    
  </div>
</template>

<script>
import Brewery from './Brewery';

import axios from 'axios';
import states from '../assets/states.json';
import types from '../assets/types.json';
import tags from '../assets/tags.json';

export default {
  name: 'BrewerySearch',
  components: {
    Brewery,
  },
  data () {
    return {
      breweryTypes: types,
      breweryTags: tags,
      selectedType: null,
      selectedTags: [null],
      city: null,
      state: null,
      statesList: states,
      requestUrl: 'https://api.openbrewerydb.org/breweries?',
      breweries: {},
      toggle: false,
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
      
      axios
        .get(this.requestUrl + options)
        .then(response => {
          this.breweries = response.data;
          this.toggle = true;
        })
    }
  }
}
</script>

<style scoped>
</style>
