<template>
  <div id="app">
    Ciao
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {
    
  },
  data: function() {
    return {
      apiUrl: 'https://api.themoviedb.org/3/search/movie',
      title: 'ritorno al futuro'
    }
  },
  created: function() {
    axios
      .get(this.apiUrl, {
        params: {
          api_key: 'cb304e29663a7b9973c26a03b4532795',
          language: 'it-IT',
          query: this.title
        }
      })
      .then (response => {
        console.log(response.data.results);
      })
      .catch()
  }
}
</script>

<style lang="scss">
  @import './assets/style/general.scss';
</style>
