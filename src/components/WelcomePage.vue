<template>
    <div class="welcome">
        <div class="welcome__card">
            <div class="welcome__card-poster">
                <img src="/src/assets/image/poster.jpeg" alt="">
            </div>

            <div class="welcome__card-start-game-button" @click="$emit('startGame', true)">
                ИГРАТЬ
            </div>
        </div>
    </div>
</template>
<script>

export default {
    name: 'WelcomePage',
}
</script>
<style scoped>

.welcome {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    min-height: 100vh;
}

.welcome__card {
    padding-top: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    border-radius: 40px;
    overflow: hidden;
    background: #fff;
    width: 50%;
    -webkit-box-shadow: 0px 1px 14px 1px rgba(34, 60, 80, 0.2);
    -moz-box-shadow: 0px 1px 14px 1px rgba(34, 60, 80, 0.2);
    box-shadow: 0px 1px 14px 1px rgba(34, 60, 80, 0.2);
}

.welcome__card-poster {
    display: flex;
    justify-content: center;
    width: 80%;
}

.welcome__card-poster img {
    width: 100%;
    height: auto;
}

.welcome__card-start-game-button {
    margin-top: 30px;
    margin-bottom: 20px;
    padding: 10px 40px 10px 40px;
    background: rgb(183, 0, 159);
    color: #fff;
    border-radius: 20px;
    font-weight: 700;
    font-size: 24px;
    cursor: pointer;
    transition: ease-in-out .2s all;
}

.welcome__card-start-game-button:hover {
    background: rgb(0, 144, 48);
}

</style>
