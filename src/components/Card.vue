<template>
    <a href="#" class="movie_card">
        <img 
            class="poster"
            :src="'https://image.tmdb.org/t/p/w342' + movie.poster_path" 
            :alt="'Immagine ' + (title? title:name)">
        <ul>
            <li>
                <strong>Titolo</strong>: 
                {{ title? title:name  }}
            </li>
            <li>
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
            <li class="d-flex">
                <strong>Voto</strong>: {{ vote }} 
                <span class="d-flex justify-content-between">
                    <i v-for="number,index in 5" :key="index" :class="enterStar" class="far fa-star"></i>
                </span>
            </li>
        </ul>
    </a>
</template>

<script>
export default {
    name: 'Card',
    props: {
        movieDetails: Object
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
            return ((this.movie.vote_average * 5) / 10).toFixed(0);
        }
    },
    methods: {
        selectImgFlag: function(language) {
            return language == 'en' || language == 'it';
        },
        selectImgSrc: function(language) {
            return language == 'en'? require('../assets/images/en.png'):require('../assets/images/it.png');
        },
        selectImgAlt: function(language) {
            return language == 'en'? 'EN':'IT';
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    .movie_card {
        position: relative;
        width: calc((100% / 5) - 40px);
        height: 328.8px;
        padding: 30px 15px 50px 15px;
        border: 1px solid lighten($bg-color, 20%);
        margin: 20px;
        background-color: $bg-color;
        transition: border .3s linear;

        .poster {
            display: block;
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            text-align: center;
            background-color: $bg-color;
            transition: opacity .3s linear;
        }

        &:hover {
            border: 1px solid $base-color;
        }

        &:hover > .poster {
            opacity: 0;
        }

        ul {
            color: darken($base-color, 40%);

            li {
                line-height: 1.13;

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
                    i.fas {
                        margin: 0 1px;
                        color: $star-color;
                    }
                }
            }
        }
    }
</style>