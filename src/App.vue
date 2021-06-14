<template>
  <div id="app">
    <!-- HEADER -->
    <!-- /HEADER -->
    <Header />

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
