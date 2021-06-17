<template>
    <form class="d-flex align-items-center">
        <div>
            <button type="submit" @click.prevent="sendSearch" @keyup.prevent="sendSearch">
                <i class="fas fa-search"></i>
            </button>
        </div>
        <div>
            <input type="text" placeholder="Inserisci titolo" v-model.trim="titleSearched" @keyup="capitalizeFirstLetter">
        </div>
    </form>
</template>

<script>
export default {
    name: 'Search',
    props: {
        selectedGenre: String
    },
    data: function() {
        return {
            titleSearched: ''
        }
    },
    computed: {
        // Se non è stato selezionato nessun genere, verrà restituita una stringa vuota
        genre: function() {
            if (this.selectedGenre == '') {
                return '';
            } else {
                return this.selectedGenre;
            }
        }
    },
    methods: {
        // Rende la prima lettera inserita nel campo di ricerca maiuscola
        capitalizeFirstLetter: function() {
            if (this.titleSearched.length == 1) {
                 this.titleSearched = this.titleSearched.toUpperCase();
            }
        },
        // Passa all'Header il titolo cercato, svuota il campo di ricerca e resetta il filtro per generi
        sendSearch: function() {
            if (this.titleSearched.length != 0) {
                this.$emit('search', this.titleSearched.toLowerCase());
                this.titleSearched = '';
                if (this.genre.length > 0) {
                    this.$emit('select', '')
                }
            }
        }
    }

}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    form {
        div {
            margin-left: 20px;

            &:first-of-type {
                margin-left: 5px;
            }

            input {
                width: 200px;
                height: 38px;
                padding: 3px 10px;
                border: unset;
                border-radius: 5px;
            }

            button {
                border: unset;
                color: $nav-link;
                background-color: unset;
                transition: color .3s linear;

                &:hover {
                    color: $base-color;
                }
            }
        }
    }
</style>