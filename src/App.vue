<template>
  <Header title="Whack-a-mole" />
  <Score ref="score" />

  <div class="center session-controls">
    <button v-if="!isGameRunning" class="session-button" @click="startGame()">Start</button>
    <button v-else class="session-button" @click="stopGame()">Stop</button>
    <GameTime
        ref="gametime"
        :time-in-seconds="10"
        @timeout="stopGame()"
    />
  </div>

  <GameBoard
      ref="gameboard"
      @score-gain="$refs.score.addToScore($event)"
      :number-burrows="numberBurrows"
  />
  <Footer/>
<!--  <Slider-->
<!--      v-model="numberBurrows"-->
<!--      :min="1"-->
<!--      :max="20"-->
<!--      :interval="1"-->
<!--      :dot-size="20"-->
<!--      class="center"-->
<!--      style="width: 300px; margin-top: 32px; background-color: forestgreen"-->
<!--  ></Slider>-->

</template>

<script>
import Header from '@/components/Header'
import Score from "@/components/Score";
import GameTime from "@/components/GameTime";
import GameBoard from "@/components/GameBoard";
import Footer from "@/components/Footer";
import {ref} from "vue";
// import Slider from 'vue-slider-component'

export default {
  name: 'App',
  components: {
    Header,
    Score,
    GameTime,
    GameBoard,
    Footer
    // Slider
  },
  data () {
    return {
      numberBurrows: 12
    }
  },
  setup () {
    const score = ref(null)
    const gametime = ref(null)
    const gameboard = ref(null)
    let isGameRunning = ref(false)

    function startGame () {
      if (!isGameRunning.value) {
        isGameRunning.value = true
        score.value.resetScore()
        gametime.value.startTimer()
        gameboard.value.startGame()
      }
    }

    function stopGame () {
      isGameRunning.value = false
      gametime.value.resetTimer()
      gameboard.value.stopGame()
    }

    return {
      score,
      gametime,
      gameboard,
      isGameRunning,
      startGame,
      stopGame
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 80px;
}

.center {
  margin-left: auto;
  margin-right: auto;
}

.session-controls {
  display: flex;
  flex-direction: row-reverse;
  width: 300px;
}

.session-button {
  border-radius: 25px;
  font-size: 24px;
  width: 100px;
  margin: 8px 8px 8px auto;
  padding: 8px;
  color: aliceblue;
  background-color: forestgreen;
}
</style>
