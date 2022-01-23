<template>
  <div class="score-container center">
    <div class="score">
      {{ currentScore }}
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: "Score",
  setup () {
    const currentScore = ref(0)

    /**
     * how many points should be added to the score
     * @param scoreGain
     */
    function addToScore (scoreGain) {
      currentScore.value = currentScore.value + scoreGain
    }

    /**
     * reset score to zero
     */
    function resetScore () {
      currentScore.value = 0
    }

    return {
      currentScore,
      addToScore,
      resetScore
    }
  }
}
</script>

<style scoped>
.score-container {
  max-width: 300px;
  border-radius: 32px;
  margin: 12px;
  border: 8px solid forestgreen;
  background-color: #301515;
}

.center {
  margin-left: auto;
  margin-right: auto;
}

.score {
  color: aliceblue;
  margin: 4px 8px 4px auto;
  text-align: right;
}

</style>