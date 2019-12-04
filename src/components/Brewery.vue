<template>
  <div class="card brewery" style="width: 18rem;">
    <div class="card-body">
      <h5 class="card-title">{{ brewery.name | lowercase }}</h5>
      <a v-bind:href="brewery.website_url" target="_blank" class="card-link" v-if="urlExists">website</a>
      <a v-bind:href="directionsUrl" target="_blank" class="card-link" v-if="directionsExist">directions</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Brewery',
  props: {
    brewery: Object,
  },
  data () {
    return {
      directionsUrl: '',
      urlExists: false,
      directionsExist: false,
    }
  },
  methods: {
    loadCard(){
      if (this.brewery.website_url !== ""){
        this.urlExists = true
      }

      if (this.brewery.street !== ""){
        this.directionsUrl = 'https://www.google.com/maps?q=' + this.brewery.street.split(' ').join('+') + '+' + this.brewery.city + '+' + this.brewery.state;
        this.directionsExist = true
      }
    }
  },
  filters: {
    lowercase: function (str) {
      if (!str) return ''
      return str.toLowerCase()
    }
  },
  mounted(){
    this.loadCard();
  }
}
</script>

<style scoped>
.brewery{
  margin-top: 10%;
  /* background-color: #6B7A8F;
  color: #F7882F; */
}
/* a:link { color: #F7C331; }
a:visited { color: #F7C331; }
a:hover { color: #F7C331; }
a:active { color: #F7C331; } */

</style>