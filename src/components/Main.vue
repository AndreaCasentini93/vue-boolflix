<template>
    <main>
        <section v-if="movies.length != 0" class="cards_box d-flex justify-content-center flex-wrap">
            <Card v-for="movie, index in movies" :key="index" :movieDetails="movie"/>
        </section>
        <section v-else-if="!homeLayout" class="not_found d-flex flex-column justify-content-center align-items-center"><div>Siamo spiacenti...</div>Nessun risultato trovato</section>
        <section v-else class="home_layout d-flex flex-column justify-content-center align-items-center text-center"><div>Benvenuto!</div>Inizia la tua ricerca</section>
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
        searchArray: Array,
        home: Boolean
    },
    computed: {
        movies: function() {
            return this.searchArray;
        },
        homeLayout: function() {
            return this.home;
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

        .cards_box {
            padding-right: 10px;
            padding-left: 10px;
        }

        .not_found {
            @include home-message(50px, 60px);
        }

        .home_layout {
            @include home-message(90px, 100px);
            padding: 0 20px;
            background: url('../assets/images/start-bg.png');
            background-size: cover;
            background-position: center;
            text-shadow: 5px 5px 8px $shadow-color;
        }
    }
</style>