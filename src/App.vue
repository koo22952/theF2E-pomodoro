<template>
    <div id="app">
        <Home></Home>
        <!--        <router-view/>-->
    </div>
</template>

<script>

import Home from './views/Home'

export default {
    components: {
        Home
    }

}
</script>
<style lang="scss">
    @import "./assets/all.scss";

    #app {
        width: 100%;
        background-color: rgba(2, 3, 4, 0.1);
        /*background: linear-gradient(*/
        /*                to right,*/
        /*                #ffedf7,*/
        /*                #ffedf7 50%,*/
        /*                #003164 75%,*/
        /*                #003164 100%*/
        /*);*/
    }
</style>
