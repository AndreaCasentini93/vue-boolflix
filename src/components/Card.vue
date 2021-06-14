<template>
    <a href="#" class="movie_card">
        <div>{{ 'Titolo: ' + movie.title }}</div>
        <div>{{ 'Titolo originale: ' + movie.original_title }}</div>
        <img v-if="selectImgFlag(movie.original_language)" :src="selectImgSrc(movie.original_language)" :alt="selectImgAlt(movie.original_language)">
        <div v-else>{{ 'Lingua originale: ' + movie.original_language.toUpperCase() }}</div>
        <div>{{ 'Voto: ' + ((movie.vote_average * 5) / 10) }}</div>
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
        width: calc((100% / 5) - 40px);
        padding: 30px 5px 50px 5px;
        margin: 20px;
        background-color: black;

        img {
        width: 23px;
        height: 23px;
        border-radius: 50%;
        object-fit: cover;
        object-position: center;
        box-shadow: 0 0 2px .5px $base-color;
        }
    }
</style>