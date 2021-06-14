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
    data: function() {
        return {
            titleSearched: ''
        }
    },
    methods: {
        capitalizeFirstLetter: function() {
            if (this.titleSearched.length == 1) {
                 this.titleSearched = this.titleSearched.toUpperCase();
            }
        },
        sendSearch: function() {
            if (this.titleSearched.length != 0) {
                this.$emit('search', this.titleSearched.toLowerCase());
                this.titleSearched = '';
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

            button {
                border: unset;
                color: darken($base-color, 50%);
                background-color: unset;
                transition: color .3s linear;

                &:hover {
                    color: $base-color;
                }
            }

            input {
                min-width: 150px;
                padding: 3px 10px;
                border: unset;
                border-radius: 5px;
            }
        }
    }
</style>