<template>
    <div class="container">
        <ul>
            <li v-for="movie, index in movies" :key="index">
                {{ movie.title }}<br>
                {{ movie.original_title }}<br>
                {{ movie.original_language }}<br>
                {{ (movie.vote_average * 5) / 10 }}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'Main',
    props: {
        searchArray: Array
    },
    computed: {
        movies: function() {
            return this.searchArray;
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';
</style>