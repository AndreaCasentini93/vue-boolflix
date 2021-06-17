<template>
  <div id="app">
    <template v-if="!loading">
      <!-- HEADER -->
      <Header @search="titleSearched" @select="genreSelected"/>
      <!-- /HEADER -->

      <!-- MAIN -->
      <Main :searchArray="movies" :home="homeLayout" :genreCall="selectedGenre"/>
      <!-- /MAIN -->
    </template>

    <!-- LOADING -->
    <Loading v-else/>
    <!-- /LOADING -->
  </div>
</template>

<script>
import axios from 'axios';
import Header from './components/Header.vue';
import Main from './components/Main.vue';
import Loading from './components/Loading.vue'

export default {
  name: 'App',
  components: {
    Header,
    Main,
    Loading
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
      loading: false,
      selectedGenre: ''
    }
  },
  computed: {
    // Compila l'array generale di film e serie televisive ordinando i titoli in ordine di popolarità
    movies: function() {
      return [...this.moviesArray, ...this.tvSeriesArray].sort(function(a, b){return b.popularity-a.popularity});
    },
  },
  methods: {
    // Effettua più chiamate all'API per recuperare film e serie TV
    titleSearched: function(val) {
      // Comandi di default
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
          // Riempe array dei film
          this.moviesArray = response.data.results;
          // Se array è vuoto blocca il loading
          if (this.moviesArray.length == 0) {
            this.moviesFound = false;
            this.loading = false;
          }

          // Cicla l'array dei film per inserire in ognuno una nuova chiave "cast" con all'interno 5 membri del cast
          let newMoviesArray = this.moviesArray;
          this.moviesArray = []
          newMoviesArray.forEach(movie => {
            movie = {...movie, cast: []};
            // Chiamata API per attori film
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

                // Solo se l'array dei film si è riempito allora verrà fatta la nuova chiamata all'API
                if (this.moviesArray.length == newMoviesArray.length) {
                  // Cicla l'array dei film per inserire in ognuno una nuova chiave "genres" con i generi corrispondenti
                  let newMoviesArray2 = this.moviesArray;
                  this.moviesArray = []
                  newMoviesArray2.forEach(movie => {
                    movie = {...movie, genres: []};
                    // Chiamata API per genere film
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
                        // Quando l'array sarà pieno, allora verrà bloccato il loading
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
          // Riempe l'array delle serie TV
          this.tvSeriesArray = response.data.results;
          // Se array è vuoto blocca il loading
          if (this.tvSeriesArray.length == 0) {
            this.tvSeriesFound = false;
            this.loading = false;
          }

          // Cicla l'array delle serie TV per inserire in ognuna una nuova chiave "cast" con all'interno 5 membri del cast
          let newTvSeriesArray = this.tvSeriesArray;
          this.tvSeriesArray = [];
          newTvSeriesArray.forEach(tvSerie => {
            tvSerie = {...tvSerie, cast: []};
            // Chiamata API per attori serie TV
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

                // Solo se l'array delle serie TV si è riempito allora verrà fatta la nuova chiamata all'API
                if (this.tvSeriesArray.length == newTvSeriesArray.length) {
                  // Cicla l'array delle serie TV per inserire in ognuna una nuova chiave "genres" con i generi corrispondenti
                  let newTvSeriesArray2 = this.tvSeriesArray;
                  this.tvSeriesArray = []
                  newTvSeriesArray2.forEach(movie => {
                    movie = {...movie, genres: []};
                    // Chiamata API per genere serie TV
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
                        // Quando l'array sarà pieno, allora verrà bloccato il loading
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
        })
        .catch(err => {
          console.log('Errore: ', err);
        })
    },
    // Salva il genere selezionato nel dato "selectedGenre"
    genreSelected: function(val) {
      this.selectedGenre = val;
    }
  }
}
</script>

<style lang="scss">
  @import './assets/style/general.scss';
</style>
