<template>
  <div id="app">
    <template v-if="!loading">
      <!-- HEADER -->
      <Header @search="titleSearched"/>
      <!-- /HEADER -->

      <!-- MAIN -->
      <Main :searchArray="movies" :home="homeLayout"/>
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
      homeLayout: true
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
          this.homeLayout = false;
          this.loading = true;
          this.movies = [];
          this.movies = response.data.results;
          this.movies = this.movies.sort(function(a, b){return b.popularity-a.popularity});
          axios
            .get(this.tvSeriesApi.apiUrl, {
              params: {
                api_key: this.tvSeriesApi.apiKey,
                language: this.tvSeriesApi.apiLanguage,
                query: this.title
              }
            })
            .then (response => {
              this.movies = [...this.movies, ...response.data.results];
              this.movies = this.movies.sort(function(a, b){return b.popularity-a.popularity});
              this.loading = false;
            })
            .catch()
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
