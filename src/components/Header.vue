<template>
    <header>
        <nav class="d-flex justify-content-between align-items-center">
            <img src="../assets/images/logo-boolflix.png" alt="Logo Boolflix">
            <Search @search="titleSearched"/>
        </nav>
    </header>
</template>

<script>
import Search from './Search.vue';

export default {
    name: 'Header',
    components: {
        Search
    },
    methods: {
        titleSearched: function(val) {
            this.$emit('search', val)
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    header {
        background-color: black;

        nav {
            padding: 20px 30px;

            img {
                width: 150px;
            }
        }
    }
</style>