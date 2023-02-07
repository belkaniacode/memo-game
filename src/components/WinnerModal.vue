<template>
    <div class="winner">
        <div class="winner__headline">
            Поздравляем!
        </div>

        <div class="winner__text">
            {{ text }}
        </div>

        <div class="new-game" @click="$emit('newGame', true)">
            Новая игра
        </div>
    </div>
</template>
<script>
import { ref, watch } from 'vue'
export default {
    name: 'WinnerModal',

    props: {
        winner: {
            default: null
        }
    },

    setup(props) {

        let text = ref('Ничья')

        const winnerWatch = watch( () => props.winner, (val) => {
            if (val) {
                text.value = `Выигрывает ${val}`
            }
        })

        return {
            text,
            winnerWatch
        }
    }
}
</script>
<style scoped>
    .winner {
        position: absolute;
        z-index: 1000;
        top: 20%;
        padding-top: 10px;
        padding-bottom: 20px;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        border-radius: 40px;
        overflow: hidden;
        background: #fff;
        width: 50%;
        margin-left: auto;
        margin-right: auto;
        left: 0;
        right: 0;
        -webkit-box-shadow: 0px 1px 14px 1px rgba(34, 60, 80, 0.2);
        -moz-box-shadow: 0px 1px 14px 1px rgba(34, 60, 80, 0.2);
        box-shadow: 0px 1px 14px 1px rgba(34, 60, 80, 0.2);
    }

    .winner__headline {
        color: rgb(183, 0, 159);
        font-size: 24px;
        font-weight: 600;
        margin-bottom: 4px;
        text-align: center;
        width: 100%;
        padding-bottom: 10px;
        border-bottom: solid 1px  rgb(242, 159, 231);
    }

    .winner__text {
        margin-top: 30px;
        color: rgb(0, 157, 29);
        font-size: 36px;
        font-weight: 600;
        margin-bottom: 4px;
        text-align: center;
    }

    .new-game {
        margin-top: 40px;
        margin-bottom: 20px;
        padding: 10px 40px 10px 40px;
        background: rgb(183, 0, 159);
        color: #fff;
        border-radius: 20px;
        font-weight: 700;
        font-size: 18px;
        cursor: pointer;
        transition: ease-in-out .2s all;
    }
    .new-game:hover {
        background: rgb(0, 144, 48);
    }
</style>
