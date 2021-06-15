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
      moviesApi: {
        apiUrl: 'https://api.themoviedb.org/3/search/movie',
        apiKey: 'cb304e29663a7b9973c26a03b4532795',
        apiLanguage: 'it-IT'
      },
      tvSeriesApi: {
        apiUrl: 'https://api.themoviedb.org/3/search/tv',
        apiKey: 'cb304e29663a7b9973c26a03b4532795',
        apiLanguage: 'it-IT'
      },
      title: '',
      movies: [],
      loading: false,
      callAxios: false
    }
  },
  methods: {
    titleSearched: function(val) {
      this.title = val;
      axios
        .get(this.moviesApi.apiUrl, {
          params: {
            api_key: this.moviesApi.apiKey,
            language: this.moviesApi.apiLanguage,
            query: this.title
          }
        })
        .then (response => {
          this.loading = true;
          this.movies = [];
          this.movies = response.data.results;
          this.movies = this.movies.sort(function(a, b){return b.vote_average-a.vote_average});
          this.callAxios = true;
          if (this.callAxios == true) {
            axios
              .get(this.tvSeriesApi.apiUrl, {
                params: {
                  api_key: this.tvSeriesApi.apiKey,
                  language: this.tvSeriesApi.apiLanguage,
                  query: this.title
                }
              })
              .then (response => {
                this.callAxios = false;
                this.movies = [...this.movies, ...response.data.results];
                this.movies = this.movies.sort(function(a, b){return b.vote_average-a.vote_average});
                this.loading = false;
              })
              .catch()
          }
        })
        .catch()
    }
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
