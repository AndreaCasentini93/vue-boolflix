<template>
    <a v-if="selectForGenres" href="#" class="movie_card">
        <!-- CARD IMAGE -->
        <img 
            v-if="movie.poster_path"
            class="poster"
            :src="'https://image.tmdb.org/t/p/w342' + movie.poster_path" 
            :alt="title? title:name">
        <img
            v-else
            class="alternative-poster" 
            src="../assets/images/logo-boolflix.png" 
            alt="Logo Boolflix">
        <!-- /CARD IMAGE -->

        <!-- CARD INFO -->
        <ul>
            <li>
                <strong>Titolo</strong>: 
                {{ title? title:name  }}
            </li>
            <li v-if="title != originalTitle || name != originalName">
                <strong>Titolo originale</strong>: 
                {{ title? originalTitle:originalName }}
            </li>
            <li>
                <strong>Lingua originale</strong>: 
                <img class="flag" 
                    v-if="selectImgFlag(language)" 
                    :src="selectImgSrc(language)" 
                    :alt="selectImgAlt(language)">
                <template v-else>{{ language.toUpperCase() }}</template>
            </li> 
            <li v-if="vote > 0" class="d-flex">
                <strong>Voto</strong>:
                <span class="d-flex justify-content-between">
                    <i v-for="number,index in 5" :key="index" :class="enterStar(index, vote)" class="fa-star"></i>
                </span>
            </li>
            <li v-if="castList() != ''">
                <strong>Cast</strong>: 
                {{ castList() }}
            </li>
            <li v-if="genresList() != ''">
                <strong>Genere</strong>: 
                {{ genresList() }}
            </li>
            <li v-if="plot">
                <strong>Trama</strong>: 
                {{ plot }}
            </li>
        </ul>
        <!-- /CARD INFO -->
    </a>
</template>

<script>
export default {
    name: 'Card',
    props: {
        movieDetails: Object,
        changeGenre: String
    },
    data: function() {
        return {
            languagesArray: [
                'en',
                'it'
            ]
        }
    },
    computed: {
        movie: function() {
            return this.movieDetails
        },
        title: function() {
            return this.movie.title;
        },
        name: function() {
            return this.movie.name;
        },
        originalTitle: function() {
            return this.movie.original_title;
        },
        originalName: function() {
            return this.movie.original_name;
        },
        language: function() {
            return this.movie.original_language;
        },
        vote: function() {
            return ((this.movie.vote_average) / 2).toFixed(0);
        },
        cast: function() {
            return this.movie.cast;
        },
        genres: function() {
            return this.movie.genres;
        },
        plot: function() {
            return this.movie.overview;
        },
        selectedGenre: function() {
            return this.changeGenre;
        },
        // La card comparirà se non sono stati selezionati generi, o se il genere selezionato fa parte della card
        selectForGenres: function() {
            if (this.selectedGenre == 'Seleziona Genere' || this.selectedGenre == '' || this.genres.includes(this.selectedGenre)) {
                return true;
            } else {
                return false;
            }
        }
    },
    methods: {
        // La bandierina comparirà se la lingua è nell'array delle lingue
        selectImgFlag: function(language) {
            return this.languagesArray.includes(language);
        },
        // Compone la stringa dell'immagine per la bandierina
        selectImgSrc: function(language) {
            if (this.languagesArray.includes(language)) {
                return require('../assets/images/' + language + '.png');
            }
        },
        // Se la lingua non è prensente nell'array delle lingue, allora l'ALT sarà un stringa
        selectImgAlt: function(language) {
            if (this.languagesArray.includes(language)) {
                return language;
            }
        },
        // Se l'indice della stellina è minore o uguale al voto, la stellina sarà piena
        enterStar: function(index, vote) {
            if ((index + 1) <= vote) {
                return 'fas';
            } else {
                return 'far';
            }
        },
        // Compone la stringa del cast
        castList: function() {
            let castString = '';
            this.cast.forEach(element => {
                if (castString == '') {
                    castString = element;
                } else {
                    castString += ', ' + element;
                }
            });
            return castString
        },
        // Compone la stringa dei generi
        genresList: function() {
            let genresString = '';
            if (this.genres) {
                this.genres.forEach(element => {
                if (genresString == '') {
                    genresString = element;
                } else {
                    genresString += ', ' + element;
                }
            });
            }
            return genresString
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    .movie_card {
        position: relative;
        width: 342px;
        height: 513px;
        border: 1px solid $bg-color;
        margin: 5px;
        background-color: $shadow-color;
        transition: border .3s linear;
        overflow: hidden;

        &:hover {
            background-color: unset;
        }

        .poster {
            display: block;
            position: absolute;
            top: 50%;
            left: 0;
            transform: translateY(-50%);
            text-align: center;
            background-color: $bg-color;
            transition: opacity .3s linear;
        }

        &:hover > .poster {
            z-index: -1;
        }

        .alternative-poster {
            display: block;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            background-color: unset;
            transition: opacity .3s linear;
        }

        &:hover > .alternative-poster {
            z-index: -1;
        }

        ul {
            width: 100%;
            height: 100%;
            padding: 20px 10px;
            color: darken($base-color, 40%);
            background-color: $movie-info;
            overflow: hidden;
            opacity: 0;
            transition: opacity .3s;

            li {
                line-height: 1.15;

                &:not(:last-child) {
                    margin-bottom: 15px;
                }

                strong {
                    color: $base-color;
                }

                .flag {
                    width: 23px;
                    height: 23px;
                    border-radius: 50%;
                    object-fit: cover;
                    object-position: center;
                    box-shadow: 0 0 2px .5px $base-color;
                }

                span {
                    margin-left: 5px;

                    i.fas {
                        margin: 0 1px;
                        color: $star-color;
                    }
                }
            }
        }

        &:hover > ul {
            overflow: auto;
            opacity: 1;
        }
    }

</style>