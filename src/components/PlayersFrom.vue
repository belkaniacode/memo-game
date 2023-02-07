<template>
    <div class="form">

        <div class="form__card">

            <div class="form__headline">
                Введите имена игроков
            </div>

            <div class="form__inputs">
                <div class="form__input">
                    <div class="form__input-label">Имя игрока №1</div>
                    <input placeholder="Введите имя" type="text" v-model="player_1">
                </div>

                <div class="form__input">
                    <div class="form__input-label">Имя игрока №2</div>
                    <input  placeholder="Введите имя" type="text" v-model="player_2">
                </div>
            </div>

            <transition name="slide-fade"> 
                <div class="form__input-button" v-show="showSubmit" @click="submit">
                    ГОТОВО
                </div>
            </transition>

        </div>

    </div>
</template>
<script>
import {ref, watch} from 'vue'
export default {
    name: 'PlayersformPage',

    setup(_, context) {
        let player_1 = ref(null)
        let player_2 = ref(null)
        let showSubmit = ref(null)

        const player_1dWatch = watch( () => player_1.value, (val) => {
           
            if (player_1.value && player_2.value) {
                showSubmit.value = true
            }else {
                showSubmit.value = false
            }
        });

        const player_2dWatch = watch( () => player_2.value, (val) => {
            if (player_1.value && player_2.value) {
                showSubmit.value = true
            }else {
                showSubmit.value = false
            }
        });

        const submit = () => {
            const players = [
                {
                    name: player_1.value,
                    counter: 0
                },
                {
                    name: player_2.value,
                    counter: 0
                }
            ]
            context.emit('onPlayerSelected', players)
        }


        return {
            player_1,
            player_2,
            showSubmit,
            player_1dWatch,
            player_2dWatch,
            submit
        }

    }

}
</script>
<style scoped>
.form {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    min-height: 100vh;
}

.form__card {
    padding-top: 30px;
    padding-bottom: 20px;
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

.form__headline {
    color: rgb(183, 0, 159);
    text-align: center;
    font-weight: 700;
    font-size: 24px;
    text-transform: uppercase;

}

.form__inputs {
    width: 100%;
    display: flex;
    justify-content: center;
    flex-direction: column;
    padding-bottom: 40px;
}

.form__input {
    margin: 0 auto;
    width: 50%;
    margin-top: 20px;
}
.form__input-label{
    color: rgb(183, 0, 159);
    font-size: 18px;
    font-weight: 600;
    margin-bottom: 4px;
    text-align: center;
}

.form__input input {
    height: 40px;
    border: solid 2px rgb(118, 101, 117) !important;
    border-radius: 20px;
    padding: 20px;
    font-size: 24px;
    font-weight: 700;
    color: rgb(79, 0, 69);
    width: 100%;
}
.form__input input::placeholder {
    color: rgb(195, 195, 195);
}

.form__input-button {
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

.form__input-button:hover {
    background: rgb(0, 144, 48);
}
</style>