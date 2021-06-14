<template>
    <a href="#" class="movie_card">
        <img 
            class="poster"
            :src="'https://image.tmdb.org/t/p/w342' + movie.poster_path" 
            :alt="movie.name == undefind? movie.title:movie.name">
        <ul>
            <li>
                <strong>Titolo</strong>: 
                {{ movie.name == undefind? movie.title:movie.name  }}
            </li>
            <li>
                <strong>Titolo originale</strong>: 
                {{ movie.original_name == undefind? movie.original_title:movie.original_name }}
            </li>
            <li>
                <strong>Lingua originale</strong>: 
                <img class="flag" 
                    v-if="selectImgFlag(movie.original_language)" 
                    :src="selectImgSrc(movie.original_language)" 
                    :alt="selectImgAlt(movie.original_language)">
                <template v-else>
                    {{ movie.original_language.toUpperCase() }}
                </template>
            </li>
            <li>
                <strong>Voto</strong>: 
                {{ ((movie.vote_average * 5) / 10).toFixed(0) }}
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
        margin: 20px;
        background-color: black;

        .poster {
            display: block;
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            transition: opacity .3s linear;
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
            }
        }
    }
</style>