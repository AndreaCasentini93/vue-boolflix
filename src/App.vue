<template>
  <div id="app">
    <template v-if="!loading">
      <!-- HEADER -->
      <Header @search="titleSearched"/>
      <!-- /HEADER -->

      <!-- MAIN -->
      <Main />
      <!-- /MAIN -->
    </template>

    <!-- LOADING -->
    <div v-else class="d-flex justify-content-center align-items-center">
      <span>Loading</span>
    </div>
    <!-- /LOADING -->
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
        apiLanguage: 'it-IT'
      },
      title: 'a',
      movieArray: [],
      loading: true
    }
  },
  methods: {
    titleSearched: function(val) {
      this.title = val;
      console.log(this.movieArray);
    }
  },
  created: function() {
    axios
      .get(this.api.apiUrl, {
        params: {
          api_key: this.api.apiKey,
          language: this.api.apiLanguage,
          query: this.title
        }
      })
      .then (response => {
        this.movieArray = response.data.results;
        this.loading = false;
      })
      .catch()
  }
}
</script>

<style lang="scss">
  @import './assets/style/general.scss';
</style>
