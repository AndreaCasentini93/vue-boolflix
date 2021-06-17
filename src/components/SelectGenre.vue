<template>
    <select class="form-select" aria-label="Default select example">
        <option selected>Seleziona Genere</option>
        <option value="Genere1">Genere1</option>
    </select>
</template>

<script>
export default {
    name: 'SelectGenre'
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    select {
        width: 200px;
        height: 35px;
        margin-left: 15px;
        color: $nav-link;
        cursor: pointer;

        &:focus {
            border: 2px solid $shadow-color;
            box-shadow: 0 0 0 1px $base-color;
        }

        option {
            color: $shadow-color;
        }
    }
</style>