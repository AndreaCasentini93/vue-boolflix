<template>
    <div class="container d-flex flex-wrap">
        <Card v-for="movie, index in movies" :key="index" :movieDetails="movie"/>
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
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

</style>