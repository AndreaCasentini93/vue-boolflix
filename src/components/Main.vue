<template>
    <main>
        <section v-if="movies.length != 0" class="container d-flex flex-wrap">
            <Card v-for="movie, index in movies" :key="index" :movieDetails="movie"/>
        </section>
        <section v-else class="not-found d-flex justify-content-center align-items-center">Nessun risultato trovato</section>
    </main>
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
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    main {
        height: 100vh;
        padding-top: 90px;
        padding-right: 30px;
        padding-left: 30px;

        .not-found {
            height: 100%;
            text-transform: uppercase;
            font-size: 50px;
            font-weight: 700;
            color: $base-color;
        }
    }
</style>