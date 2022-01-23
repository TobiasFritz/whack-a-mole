<template>
  <div class="board center">
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[0] = $event" :hasMole="hasMole[0]" :active="numberBurrows > 0"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[1] = $event" :hasMole="hasMole[1]" :active="numberBurrows > 1"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[2] = $event" :hasMole="hasMole[2]" :active="numberBurrows > 2"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[3] = $event" :hasMole="hasMole[3]" :active="numberBurrows > 3"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[4] = $event" :hasMole="hasMole[4]" :active="numberBurrows > 4"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[5] = $event" :hasMole="hasMole[5]" :active="numberBurrows > 5"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[6] = $event" :hasMole="hasMole[6]" :active="numberBurrows > 6"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[7] = $event" :hasMole="hasMole[7]" :active="numberBurrows > 7"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[8] = $event" :hasMole="hasMole[8]" :active="numberBurrows > 8"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[9] = $event" :hasMole="hasMole[9]" :active="numberBurrows > 9"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[10] = $event" :hasMole="hasMole[10]" :active="numberBurrows > 10"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[11] = $event" :hasMole="hasMole[11]" :active="numberBurrows > 11"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[12] = $event" :hasMole="hasMole[12]" :active="numberBurrows > 12"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[13] = $event" :hasMole="hasMole[13]" :active="numberBurrows > 13"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[14] = $event" :hasMole="hasMole[14]" :active="numberBurrows > 14"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[15] = $event" :hasMole="hasMole[15]" :active="numberBurrows > 15"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[16] = $event" :hasMole="hasMole[16]" :active="numberBurrows > 16"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[17] = $event" :hasMole="hasMole[17]" :active="numberBurrows > 17"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[18] = $event" :hasMole="hasMole[18]" :active="numberBurrows > 18"/>
    <MoleBurrow @hit="calcScoreGain" @update:hasMole="hasMole[19] = $event" :hasMole="hasMole[19]" :active="numberBurrows > 19"/>
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
    const hasMole = ref([
      false, false, false, false,
      false, false, false, false,
      false, false, false, false,
      false, false, false, false,
      false, false, false, false,
    ])
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
      hasMole.value = [
        false, false, false, false,
        false, false, false, false,
        false, false, false, false,
        false, false, false, false,
        false, false, false, false,
      ]
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