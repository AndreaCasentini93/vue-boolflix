<template>
    <main>
        <!-- CARDS -->
        <section v-if="movies.length != 0" class="cards_box d-flex justify-content-center flex-wrap">
            <Card v-for="movie, index in movies" :key="index" :movieDetails="movie" :changeGenre="selectedGenre"/>
        </section>
        <!-- /CARDS -->

        <!-- NOT FOUND -->
        <section v-else-if="!homeLayout" class="not_found d-flex flex-column justify-content-center align-items-center"><div>Siamo spiacenti...</div>Nessun risultato trovato</section>
        <!-- /NOT FOUND -->

        <!-- HOMEPAGE -->
        <section v-else class="home_layout">
            <h2 class="d-flex flex-column justify-content-center align-items-center text-center"><div>Benvenuto!</div>Inizia la tua ricerca</h2>
        </section>
        <!-- /HOMEPAGE -->
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
        home: Boolean,
        genreCall: String
    },
    computed: {
        // Cicla l'array dei film e serie TV per ottenerne uno nuovo contenente solo i titoli con il genere selezionato
        movies: function() {
            let array = [];
            this.searchArray.forEach(element => {
                if (this.selectedGenre == '' || this.selectedGenre == 'Seleziona Genere' || element.genres.includes(this.selectedGenre)) {
                    array.push(element);
                }
            });
            return array;
        },
        homeLayout: function() {
            return this.home;
        },
        selectedGenre: function() {
            return this.genreCall;
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
            width: 100%;
            height: 100%;
            background: url('../assets/images/start-bg.png');
            background-size: cover;
            background-position: center;

            h2 {
                @include home-message(90px, 100px);
                padding: 0 20px;
                background-color: rgba($bg-color, .7);
                text-shadow: 5px 5px 8px $shadow-color;
            }
        }
    }
</style>