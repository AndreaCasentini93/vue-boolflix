<template>
    <div class="container">
        {{movieArray[0].title}}
    </div>
</template>

<script>
export default {
    name: 'Main',
    props: {
        searchArray: Array
    },
    computed: {
        movieArray: function() {
            return this.searchArray;
        }
    }
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';
</style>