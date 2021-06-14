<template>
  <div id="app">
    <template v-if="!loading">
      <!-- HEADER -->
      <Header @search="titleSearched"/>
      <!-- /HEADER -->

      <!-- MAIN -->
      <Main :searchArray="movies"/>
      <!-- /MAIN -->
    </template>

    <!-- LOADING -->
    <div v-else class="d-flex justify-content-center align-items-center debug">
      Loading
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
      movies: [],
      loading: true
    }
  },
  methods: {
    titleSearched: function(val) {
      this.loading = true;
      this.title = val;
      axios
        .get(this.api.apiUrl, {
          params: {
            api_key: this.api.apiKey,
            language: this.api.apiLanguage,
            query: this.title
          }
        })
        .then (response => {
          this.movies = response.data.results;
          this.loading = false;
        })
        .catch()
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
        this.movies = response.data.results;
        this.loading = false;
      })
      .catch()
  }
}
</script>

<style lang="scss">
  @import './assets/style/general.scss';

  .debug {
    height: 100vh;
    font-size: 50px;
  }
</style>
