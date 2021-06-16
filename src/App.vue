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
      moviesArray: [],
      tvSeriesArray: [],
      homeLayout: true,
      moviesFound: true,
      tvSeriesFound: true
    }
  },
  computed: {
    movies: function() {
      return [...this.moviesArray, ...this.tvSeriesArray].sort(function(a, b){return b.popularity-a.popularity});
    },
    loading: function(){
      if (this.movies.length == 0 && this.homeLayout) {
        return false;
      } else if ((this.moviesArray.length == 0 || this.tvSeriesArray.length == 0) && (this.moviesFound && this.tvSeriesFound)) {
        return true;
      } else {
        return false
      }
    }
  },
  methods: {
    titleSearched: function(val) {
      this.title = val;
      this.homeLayout = false;
      this.moviesArray = [];
      this.tvSeriesArray = [];
      // 1° Axios Call
      axios
        .get(this.moviesApi.apiUrl, {
          params: {
            api_key: this.moviesApi.apiKey,
            language: this.moviesApi.apiLanguage,
            query: this.title
          }
        })
        .then (response => {
          this.moviesArray = response.data.results;
          if (this.moviesArray.length == 0) {
            this.moviesFound = false;
          }
          // this.movies.forEach(movie => {
          //   movie = {...movie, cast: []};
          //   // 2° Axios Call
          //   axios
          //     .get('https://api.themoviedb.org/3/movie/' + movie.id + '/credits', {
          //       params: {
          //         api_key: this.moviesApi.apiKey,
          //         language: this.moviesApi.apiLanguage,
          //       }
          //     })
          //     .then (response => {
          //       for (let i = 0; i < 5; i++) {
          //         if (response.data.cast[i].name) {
          //           movie.cast.push(response.data.cast[i].name);
          //         }
          //       }
          //       console.log(movie);
          //       console.log(movie.title);
          //       console.log('--------------------------------------------------------------------------');
          //     })
          //     .catch(err => {
          //        console.log('Errore: ', err);
          //      })
          // });
        })
        .catch(err => {
          console.log('Errore: ', err);
        })
      axios
        .get(this.tvSeriesApi.apiUrl, {
          params: {
            api_key: this.tvSeriesApi.apiKey,
            language: this.tvSeriesApi.apiLanguage,
            query: this.title
          }
        })
        .then (response => {
          this.tvSeriesArray = response.data.results;
          if (this.tvSeriesArray.length == 0) {
            this.tvSeriesFound = false;
          }
        })
        .catch(err => {
          console.log('Errore: ', err);
        })
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
