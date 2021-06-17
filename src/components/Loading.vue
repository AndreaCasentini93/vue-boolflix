<template>
    <section class="d-flex flex-column justify-content-center align-items-center">
        <img src="../assets/images/logo-boolflix.png" alt="Logo Boolflix">
        <i class="fas fa-spinner"></i>
        <div>Loading...</div>
    </section>
</template>

<script>
export default {
    name: 'Loading'
}
</script>

<style lang="scss" scoped>
    @import '../assets/style/variables.scss';
    @import '../assets/style/mixins.scss';

    section {
        height: 100vh;
        text-transform: uppercase;
        font-size: 40px;

        img {
            width: 30%;
        }

        i {
            margin: 40px;
            font-size: 50px;
            animation: rotation 1s infinite linear;
        }
        @keyframes rotation {
            from {
                transform: rotate(0deg);
            }
            to {
                transform: rotate(360deg);
            }
        }
    }
</style>