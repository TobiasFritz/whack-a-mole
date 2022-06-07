<template>
  <div class="board center">
    <MoleBurrow
        v-for="n in hasMole.length"
        :key="n-1"
        @hit="calcScoreGain"
        @update:hasMole="hasMole[n-1] = $event"
        :hasMole="hasMole[n-1]"
        :active="numberBurrows > (n-1)"
    />
  </div>
</template>

<script>
import { ref } from 'vue'
import MoleBurrow from "@/components/MoleBurrow";

export default {
  name: "GameBoard",
  components: {
    MoleBurrow
  },
  props: {
    numberBurrows: {
      type: Number,
      default: 12
    }
  },
  emits: ['score-gain'],
  setup (props, { emit }) {
    const hasMole = ref(Array(props.numberBurrows).fill(false))
    const isGameRunning = ref(false)
    const timer = ref(null)

    /**
     * some calculations on how many points should be awarded
     */
    function calcScoreGain () {
      emit('score-gain', 100)
    }

    function startGame () {
      isGameRunning.value = true
      timer.value = setInterval(gameStep, 1000)
    }

    /**
     * check if game is still running
     * if it is then spawn a new mole
     * otherwise stop the timer
     */
    function gameStep () {
      if (isGameRunning.value) {
        spawnMole()
      } else {
        clearInterval(timer.value)
      }
    }

    /**
     * spawn a mole in a random burrow
     */
    function spawnMole () {
      if (canSpawnMole()) {
        const hasMoleIndex = Math.floor(Math.random() * (props.numberBurrows))
        if (hasMole.value[hasMoleIndex] === false) {
          hasMole.value[hasMoleIndex] = true
        } else {
          spawnMole()
        }
      }
    }

    /**
     * check if there's still free space left for more moles
     * @returns {boolean}
     */
    function canSpawnMole () {
      let canSpawn = false
      hasMole.value.forEach(hasMole => {
        if (!hasMole) {
          canSpawn = true
        }
      })
      return canSpawn
    }

    /**
     * stop current game and reset game board
     */
    function stopGame () {
      isGameRunning.value = false
      hasMole.value = Array(props.numberBurrows).fill(false)
    }

    return {
      hasMole,
      calcScoreGain,
      startGame,
      stopGame
    }
  }
}
</script>

<style scoped>
.board {
  display: flex;
  flex-wrap: wrap;
  max-width: 300px;
  margin: 16px;
  border-radius: 32px;
  border: 8px solid forestgreen;
  background-color: #301515;
}

.center {
  margin-left: auto;
  margin-right: auto;
}

</style>