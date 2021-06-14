<template>
  <div id="app">
    <!-- HEADER -->
    <Header />
    <!-- /HEADER -->

    <!-- MAIN -->
    <Main />
    <!-- /MAIN -->
  </div>
</template>

<script>
import axios from 'axios';
import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default {
  name: 'App',
  components: {
    Header,
    Main
  },
  data: function() {
    return {
      api: {
        apiUrl: 'https://api.themoviedb.org/3/search/movie',
        apiKey: 'cb304e29663a7b9973c26a03b4532795',
        apiLanguage: 'it-IT',
        apiQuery: 'ritorno al futuro'
      },
      movieArray: []
    }
  },
  created: function() {
    axios
      .get(this.api.apiUrl, {
        params: {
          api_key: this.api.apiKey,
          language: this.api.apiLanguage,
          query: this.api.apiQuery
        }
      })
      .then (response => {
        this.movieArray = response.data.results;
      })
      .catch()
  }
}
</script>

<style lang="scss">
  @import './assets/style/general.scss';
</style>
