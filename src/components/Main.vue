<template>
    <div class="container">
        <Card />
        <ul>
            <li v-for="movie, index in movies" :key="index">
                <div>{{ movie.title }}</div>
                <div>{{ movie.original_title }}</div>
                <img v-if="movie.original_language == 'en' || movie.original_language == 'it'" src="" alt="">
                <div v-else>{{ movie.original_language }}</div>
                <div>{{ (movie.vote_average * 5) / 10 }}</div>
            </li>
        </ul>
    </div>
</template>

<script>
import Card from './Card.vue'

export default {
    name: 'Main',
    components: {
        Card
    },
    props: {
        searchArray: Array
    },
    computed: {
        movies: function() {
            return this.searchArray;
        }
    },
    methods: {
        languageFlag: function(language) {
            language == 'en' || language == 'it';
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';
</style>