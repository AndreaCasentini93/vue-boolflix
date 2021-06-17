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
      tvSeriesFound: true,
      loading: false
    }
  },
  computed: {
    movies: function() {
      return [...this.moviesArray, ...this.tvSeriesArray].sort(function(a, b){return b.popularity-a.popularity});
    },
  },
  methods: {
    titleSearched: function(val) {
      this.loading = true;
      this.title = val;
      this.homeLayout = false;
      this.moviesArray = [];
      this.tvSeriesArray = [];
      this.moviesFound = true;
      this.tvSeriesFound = true;
      
      // Chiamata API per film
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
            this.loading = false;
          }

          // Chiamata API per attori film
          let newMoviesArray = this.moviesArray;
          this.moviesArray = []
          newMoviesArray.forEach(movie => {
            movie = {...movie, cast: []};
            axios
              .get('https://api.themoviedb.org/3/movie/' + movie.id + '/credits', {
                params: {
                  api_key: this.moviesApi.apiKey,
                  language: this.moviesApi.apiLanguage,
                }
              })
              .then (response => {
                for (let i = 0; i < response.data.cast.length && i < 5; i++) {
                  movie.cast.push(response.data.cast[i].name);
                }
                this.moviesArray.push(movie);
                if (this.moviesArray.length == newMoviesArray.length) {
                  
                  // Chiamata API per genere film
                  let newMoviesArray2 = this.moviesArray;
                  this.moviesArray = []
                  newMoviesArray2.forEach(movie => {
                    movie = {...movie, genres: []};
                    axios
                      .get('https://api.themoviedb.org/3/movie/' + movie.id, {
                        params: {
                          api_key: this.moviesApi.apiKey,
                          language: this.moviesApi.apiLanguage,
                        }
                      })
                      .then (response => {
                        for (let i = 0; i < response.data.genres.length; i++) {
                          movie.genres.push(response.data.genres[i].name);
                        }
                        this.moviesArray.push(movie);
                        if (this.moviesArray.length == newMoviesArray2.length) {
                          this.loading = false;
                        }
                      })
                      .catch(err => {
                        console.log('Errore: ', err);
                      })
                  });

                }
              })
              .catch(err => {
                 console.log('Errore: ', err);
               })
          });

        })
        .catch(err => {
          console.log('Errore: ', err);
        })
      
      // Chiamata API per serie TV
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
            this.loading = false;
          }

          // Chiamata API per attori serie TV
          let newTvSeriesArray = this.tvSeriesArray;
          this.tvSeriesArray = []
          newTvSeriesArray.forEach(tvSerie => {
            tvSerie = {...tvSerie, cast: []};
            axios
              .get('https://api.themoviedb.org/3/tv/' + tvSerie.id + '/credits', {
                params: {
                  api_key: this.tvSeriesApi.apiKey,
                  language: this.tvSeriesApi.apiLanguage,
                }
              })
              .then (response => {
                for (let i = 0; i < response.data.cast.length && i < 5; i++) {
                  tvSerie.cast.push(response.data.cast[i].name);
                }
                this.tvSeriesArray.push(tvSerie);
                if (this.tvSeriesArray.length == newTvSeriesArray.length) {

                  // Chiamata API per genere film
                  let newTvSeriesArray2 = this.tvSeriesArray;
                  this.tvSeriesArray = []
                  newTvSeriesArray2.forEach(movie => {
                    movie = {...movie, genres: []};
                    axios
                      .get('https://api.themoviedb.org/3/tv/' + movie.id, {
                        params: {
                          api_key: this.moviesApi.apiKey,
                          language: this.moviesApi.apiLanguage,
                        }
                      })
                      .then (response => {
                        for (let i = 0; i < response.data.genres.length; i++) {
                          movie.genres.push(response.data.genres[i].name);
                        }
                        this.tvSeriesArray.push(movie);
                        if (this.tvSeriesArray.length == newTvSeriesArray2.length) {
                          this.loading = false;
                        }
                      })
                      .catch(err => {
                        console.log('Errore: ', err);
                      })
                  });

                }
              })
              .catch(err => {
                 console.log('Errore: ', err);
               })
          });
          
          // Chiamata API per genere serie TV

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
