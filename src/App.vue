<template>
    <div class="container">
        <h1 class="text-center fw-bold font-monospace text-gray mb-5">
            Search posts by author
        </h1>
        <Posts />
    </div>
</template>

<script>
import Posts from './components/Posts.vue'

export default {
    name: 'App',
    components: { Posts }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    /* text-align: center; */
    color: #2c3e50;
    margin-top: 60px;
}
</style>
