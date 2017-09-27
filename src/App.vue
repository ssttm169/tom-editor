<template>
    <div id="app">
        <router-view></router-view>
    </div>
</template>

<script>
export default {
    name: 'app'
}
</script>


<style>
* {
    margin: 0;
    padding: 0;
}

html * {
    box-sizing: border-box;
}
li {
    list-style: none;
}

body {
    overflow-x: hidden;
    overflow-y: scroll;
    word-break: break-all;
    margin: 0;
}
</style>


