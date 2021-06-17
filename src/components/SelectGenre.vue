<template>
    <select @change="changeGenre" class="form-select" aria-label="Default select example" v-model="selectedGenre">
        <option selected>Seleziona Genere</option>
        <option v-for="genres, index in allGenresArray" :key="index" :value="genres">{{ genres }}</option>
    </select>
</template>

<script>
import axios from 'axios'

export default {
    name: 'SelectGenre',
    data: function() {
        return {
            moviesGenresApi: {
                apiUrl: 'https://api.themoviedb.org/3/genre/movie/list',
                apiKey: 'cb304e29663a7b9973c26a03b4532795',
                apiLanguage: 'it-IT'
            },
            tvSeriesGenresApi: {
                apiUrl: 'https://api.themoviedb.org/3/genre/tv/list',
                apiKey: 'cb304e29663a7b9973c26a03b4532795',
                apiLanguage: 'it-IT'
            },
            moviesGenresArray: [],
            tvSeriesGenresArray: [],
            selectedGenre: 'Seleziona Genere'
        }
    },
    computed: {
        // Genere un array con tutti i generi esistenti per film e serie TV
        allGenresArray: function() {
            let array = [...this.moviesGenresArray, ...this.tvSeriesGenresArray].sort();
            let newArray = [];
            if (array.length > this.moviesGenresArray.length && array.length > this.tvSeriesGenresArray.length) {
                array.forEach (element => {
                    if (!newArray.includes(element)) {
                        newArray.push(element);
                    }
                })
            }
            return newArray;
        }
    },
    methods: {
        // Passa all'Header il genere scelto
        changeGenre: function() {
            this.$emit('genreCall', this.selectedGenre);
        }
    },
    created: function() {
        // Chiamata all'API per ottenere tutti i generi dei film che vengono inseriti in un array
        axios
            .get(this.moviesGenresApi.apiUrl, {
            params: {
                api_key: this.moviesGenresApi.apiKey,
                language: this.moviesGenresApi.apiLanguage,
            }
            })
            .then(response => {
                response.data.genres.forEach(genres => {
                    this.moviesGenresArray.push(genres.name);
                });
            })
            .catch(err => {
                console.log('Errore: ', err);
            })

        // Chiamata all'API per ottenere tutti i generi delle serie TV che vengono inseriti in un array
        axios
            .get(this.tvSeriesGenresApi.apiUrl, {
            params: {
                api_key: this.tvSeriesGenresApi.apiKey,
                language: this.tvSeriesGenresApi.apiLanguage,
            }
            })
            .then(response => {
                response.data.genres.forEach(genres => {
                    this.tvSeriesGenresArray.push(genres.name);
                });
            })
            .catch(err => {
                console.log('Errore: ', err);
            })

    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    select {
        width: 200px;
        height: 38px;
        margin-left: 15px;
        color: $nav-link;
        cursor: pointer;

        &:focus {
            border: 2px solid $shadow-color;
            box-shadow: 0 0 0 1px $base-color;
        }

        option {
            color: $shadow-color;
        }
    }
</style>