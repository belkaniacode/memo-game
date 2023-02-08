<template>
   
    <div class="card" @click="onClickCard()">
        <div class="card__front" :class="active ? 'card__front-active' : ''">
            <img :src="image" alt="">
        </div>
        <div class="card__back" :class="active ? 'card__back-active' : ''">
            <img src="../assets/image/cards/back.jpeg" alt="">
        </div>
    </div>
    
</template>
<script>
import { computed, ref, onMounted, watch } from 'vue'
export default {
    name: 'CardComponent',

    props: {
        card: {
            type: Number,
            default: null
        },

        index: {
            type: Number,
            default: null
        },

        flipped: {
            type: Boolean,
            default: false
        },

        canClick: {
            type: Boolean,
            default: false
        }
    },


    setup(props, context) {
        

        let active = ref(false)
        let canClick = ref(false)

        const image = computed( () => {
            return `/memo/game/assets/image/cards/${props.card}.jpeg`
        })

        const onClickCard = () => {
            if (!canClick.value) return false
            rotate()
        } 

        const rotate = () => {
            if (active.value == false) active.value = true
            context.emit('onSelected',  props.index, props.card, active.value) 
        }

        const flippedWatch = watch( () => props.flipped, (val) => {
            active.value = val
        });

        const canClickWatch = watch( () => props.canClick, (val) => {
            canClick.value = val
        });

        onMounted( () => {
            active.value = props.flipped
        })

        return {
            rotate,
            flippedWatch,
            canClickWatch,
            onClickCard,
            active,
            image,
            
        }
    }
}
</script>
<style scoped>

.card {
    width: 100%;
    height: 100%;
    margin: 5px 10px 5px 10px;
    cursor: pointer;
    position: relative;
}

.card__back, .card__front {

    border-radius: 8px;
    overflow: hidden;
    backface-visibility: hidden;
    width: 100%;
    height: auto;
    transition: transform .6s linear;
}

.card__back {
    z-index: 2;
    position: relative;
}

.card__front {
    z-index: 1;
    position: absolute;
    top: 0;
    transform: perspective(600px) rotateY(180deg);
    
}

.card__back-active {
    transform: perspective(600px) rotateY(180deg);
}
.card__front-active {
    transform: perspective(600px) rotateY(0deg);
}

.card__back img, .card__front img {

    -webkit-box-shadow: 0px 0px 6px 0px rgba(0, 0, 0, 0.25);
    -moz-box-shadow: 0px 0px 6px 0px rgba(0, 0, 0, 0.25);
    box-shadow: 0px 0px 6px 0px rgba(0, 0, 0, 0.25);

    border-radius: 8px;
    width: 100%;
    height: auto;
}


</style>