<template>
    <header>
        <nav class="d-flex justify-content-between align-items-center">
            <!-- NAVBAR LEFT -->
            <div class="navbar_left d-flex justify-content-between align-items-center">
                <a href="#">
                    <img src="../assets/images/logo-boolflix.png" alt="Logo Boolflix">
                </a>
                <ul class="d-flex">
                    <li v-for="link, index in menu" :key="index">
                        <a href="#">{{ link }}</a>
                    </li>
                </ul>
            </div>
            <!-- /NAVBAR LEFT -->

            <!-- NAVBAR RIGHT -->
            <div class="navbar_right d-flex align-items-center">
                <Search @search="titleSearched"/>
                <ul class="d-flex align-items-center">
                    <li>
                        <a href="#" class="uppercase">BAMBINI</a>
                    </li>
                    <li>
                        <a href="#">
                            <i class="fas fa-bell"></i>
                        </a>
                    </li>
                    <li>
                        <a href="#" class="d-flex align-items-center">
                            <img class="netflix_profile" src="../assets/images/profilo-netflix.png" alt="Profilo Netflix">
                            <i class="fas fa-chevron-down"></i>
                        </a>
                    </li>
                </ul>
            </div>
            <!-- /NAVBAR RIGHT -->
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
    data: function() {
        return {
            menu: [
                'Home',
                'Serie Tv',
                'Film',
                'Originali',
                'Aggiunti di recente',
                'La mia lista'
            ]
        }
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
            padding: 20px 50px;

            .navbar_left {
                a {
                    img {
                        width: 150px;
                        margin-right: 50px;
                        transition: filter .3s;

                        &:hover {
                            filter: brightness(180%);
                        }
                    }
                }

                ul {
                    li {
                        margin-right: 20px;

                        a {
                            color: $nav-link;
                            cursor: pointer;
                            transition: color .3s linear;

                            &:hover {
                                color: $base-color;
                            }
                        }
                    }
                }
            }

            .navbar_right {
                ul {
                    li {
                        margin-left: 20px;
                        color: $nav-link;
                        transition: color .3s linear;

                        &:hover {
                            color: $base-color;
                        }

                        a {
                            &:hover > .netflix_profile {
                            filter: brightness(150%);
                        }

                            .netflix_profile {
                                width: 35px;
                                margin-right: 5px;
                                transition: filter .3s linear;
                            }
                        }
                    }
                }
            }
        }
    }
</style>