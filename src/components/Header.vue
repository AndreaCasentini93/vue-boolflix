<template>
    <header>
        <nav class="d-flex justify-content-between align-items-center">
            <a href="#">
                <img src="../assets/images/logo-boolflix.png" alt="Logo Boolflix">
            </a>
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
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 90px;
        background-color: $bg-color;
        z-index: 1;

        nav {
            padding: 20px 30px;

            a {
                img {
                    width: 150px;
                    transition: filter .3s;

                    &:hover {
                        filter: brightness(180%);
                    }
                }
            }
        }
    }
</style>