<template>
    <div id="main-banner">
        <h1>Make Your Burger</h1>
    </div>
</template>

<script>
export default {
    name: 'Banner'
}
</script>

<style scoped>
#main-banner {
    background-image: url('../../public/img/burger.jpg');
    background-position: center right;
    background-repeat: no-repeat;
    background-size: cover;
    display: flex;
    height: 95vh;
    align-items: center;
    justify-content: flex-start;
}

#main-banner h1 {
    color: #FFF;
    text-align: center;
    font-size: 4vw;
    background-color: #222;
    padding: 20px 40px;
}
</style>