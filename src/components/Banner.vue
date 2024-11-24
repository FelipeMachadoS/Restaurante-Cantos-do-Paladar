<template>
    <div id="main-banner">
        <h1>Canto do Paladar</h1>
    </div>
</template>

<script>
export default {
    name: "Banner"
}
</script>

<style scoped>
#main-banner {
    background-image: url('../../public/img/burger.jpg');
    background-position: 0 65%;
    background-size: cover;
    height: 200px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    border-bottom: 1px solid #111;
}

#main-banner h1 {
    color: #FFF;
    text-align: center;
    font-size: 40px;
    background-color: #222;
    padding: 20px 40px;
    margin: 0px;
}
</style>