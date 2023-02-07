<template>
  <div class="page">

    <div class="welcomePage" v-show="!showGameContainer">
      <transition name="slide-fade"> 
        <div v-show="showWelcomePage">
            <Welcome 
              @startGame="startGame"
            />
        </div>
      </transition>

      <transition name="slide-fade"> 
        <div v-show="showPlayersFrom">
            <PlayersFrom
              @onPlayerSelected="onPlayerSelected"
            />
        </div>
      </transition>

    </div>

    <div class="cards" v-show="showGameContainer">

      <transition name="slide-fade"> 
        <WinnerModal v-show="showFinish"
          :winner="winner"
          @newGame="newGame"
        />
      </transition>

      <div class="cards__indicators-container" v-if="players">
        <div class="cards__indicator" v-if="players && players[0]">{{ players[0].name }} : {{ players[0].counter }}</div>
        <div class="cards__current-player" v-if="players">
          <span>Сейчас ходит <span class="playerName">{{ players[activePlayer].name }}</span></span>
        </div>
        <div class="cards__indicator" v-if="players && players[1]">{{ players[1].counter }} : {{ players[1].name }}</div>
      </div>

      <div class="cards__container">

        <div class="cards__item" 
          v-for="(card, i) in cards" :key="i"
        >

          <Card 
            v-show="!card.disabled"
            :card="card.type"
            :flipped="card.flipped"
            :index="card.index"
            :canClick="canClick"
            @onSelected="onSelected"
          />

        </div>

      </div>
    </div>


  </div>
</template>
<script >
import { ref, onMounted, reactive} from 'vue';
import Welcome from './components/WelcomePage.vue'
import WinnerModal from './components/WinnerModal.vue'
import Card from './components/Card.vue'
import PlayersFrom from './components/PlayersFrom.vue'
export default {
  name: 'Main',

  components: {
    Welcome,
    PlayersFrom,
    Card,
    WinnerModal
  },

  setup() {
    let showWelcomePage = ref(false)
    let showGameContainer = ref(false)
    let showPlayersFrom = ref(false)
    let cards = reactive([])
    let clickMemory = null
    let activeCard = null
    let canClick = ref(false)
    let players = ref(null)
    let activePlayer = ref(0)
    let showFinish = ref(false)
    let winner = ref(null)
    const cardsCount = 18

    const start = () => {
      setTimeout(() => {
        showWelcomePage.value = true
      }, 1000)
    }

    const startGame = () => {
      showWelcomePage.value = false

      setTimeout(() => {
        showPlayersFrom.value = true
      }, 1000)
      
    }
    const onPlayerSelected = (val) => {
      players.value = val
      showPlayersFrom.value = false

      setTimeout(() => {
       showGameContainer.value = true
        render()
      }, 1000)
    }

    const cardsGenerator = () => {

      let arr = [], arrCopy = []
      for (let i = 1; i <= Math.round(cardsCount / 2); i++) {
        arr.push(i)
        arrCopy.push(i)
      }

      arr = arr.concat(arrCopy)
      arr.sort(() => Math.round(Math.random() * 100) - 50)
      arr.map((el, i) => cards.push({
        type: el,
        flipped: false,
        index: i,
        disabled: false
      }))
      
    }

    const shortShow = () => {
      cards.map((el, i) => {
        
        setTimeout(() => {
          el.flipped = true
        }, 50 * i)

      })
    }

    const closeAll = () => {
      cards.map((el, i) => {
        el.flipped = false
      })
    }

    const render = () => {
      showFinish.value = false
      winner.value = null
      canClick.value = false
      cardsGenerator()
      shortShow()

      setTimeout(() => {
        closeAll()
        canClick.value = true
      }, 1800)
    }

    const newGame = () => {
      activeCard = null
      clickMemory = null
      while(cards.length > 0) {cards.pop();}

      setTimeout(() => {
        players.value.map(el => {
          el.counter = 0
        })
        render()
      }, 100)
    }

    const changePlayer = () => {
      if (activePlayer.value == 0) activePlayer.value = 1 
      else activePlayer.value = 0
    }

    const counterIncrement = () => {
      players.value[activePlayer.value].counter ++

      let finish = true
      cards.map(el => {
        if (!el.disabled) finish = false
      })

      if (finish) {

        if (players.value[0].counter == players.value[1].counter) {
          winner.value = null
        }else if (players.value[0].counter > players.value[1].counter) {
          winner.value = players.value[0].name
        }else {
          winner.value = players.value[1].name
        }

        showFinish.value = true
      }

    }

    const onSelected = (index, type, val) => {
      
      canClick.value = false
      if (index == clickMemory) {
        canClick.value = true
        return
      }
      else clickMemory = index
      const card = cards.find(f => f.index == index)
      card.flipped = true

      if (!activeCard) {
        activeCard = type
        canClick.value = true
        return
      }

      if (activeCard === type) {

        activeCard = null
        setTimeout(() => closeAll() , 1000)

        setTimeout(() => {
          cards.map(el => {
            if (el.type == type) el.disabled = true
          })
          canClick.value = true
          counterIncrement()
        }, 1600)

      }else {
        
        activeCard = null
        setTimeout(() =>  {
          closeAll()
          canClick.value = true
          changePlayer()
        }, 1000)
      }
    }

    onMounted( () => { start() })

    return {
      start,
      cards,
      showWelcomePage,
      showGameContainer,
      onSelected,
      canClick,
      startGame,
      showPlayersFrom,
      onPlayerSelected,
      players,
      activePlayer,
      winner,
      showFinish,
      newGame
    }

  }
}

</script>
<style >

.cards {
  background-image: url('/src/assets/image/cards-back.jpeg');
  background-size: cover;
  min-height: 100vh;
}

.cards__container {
  padding-top: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}

.cards__item {
  width: 11%;
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
}

.page {
  height: 100%;
  width: 100%;
  min-height: 100vh;
}

.cards__indicators-container {
  background: rgb(183, 0, 159);
  display: flex;
  justify-content: space-between;
  width: 100%;
  padding: 10px 20px 10px 20px;
}

.cards__indicator {
  display: flex;
  align-items: center;
  font-size: 24px;
  color: #fff;
  font-weight: 700;
}

.cards__current-player {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  font-weight: 700;
  color: #fff;
}

.playerName {
  font-weight: 800;
  color: rgb(255, 242, 0);
}

.welcomePage {
  background-image: url('/src/assets/image/welcome-back.jpeg');
  background-size: cover;
  min-height: 100vh;
}

.slide-fade-enter-active {
  animation: bounce-in .4s;
}
.slide-fade-leave-active {
  animation: bounce-in .5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}

</style>