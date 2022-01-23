<template>
  <div class="gametime">
    {{ timeDisplayContent }}
  </div>
</template>

<script>
import {ref} from "vue";

export default {
  name: "GameTime",
  props: {
    timeInSeconds: {
      type: Number,
      default: 60
    }
  },
  emits: ['timeout'],
  setup (props, { emit }) {
    const remainingTimeInMilliseconds = ref(props.timeInSeconds * 1000)
    const timeDisplayContent = ref('--:--:---')
    const timer = ref(null)

    /**
     * starts the timer and updates displayed remaining time every 10 milliseconds
     * emits timeout event once remaining time hits zero
     */
    function startTimer () {
      timeDisplayContent.value = getTimeDisplayContent(remainingTimeInMilliseconds.value)
      timer.value = setInterval(
          () => {
            if (remainingTimeInMilliseconds.value <= 0) {
              emit('timeout')
            } else {
              remainingTimeInMilliseconds.value = remainingTimeInMilliseconds.value - 10
              timeDisplayContent.value = getTimeDisplayContent(remainingTimeInMilliseconds.value)
            }
          },
          10
      )
    }

    /**
     * resets timer and display to starting values
     */
    function resetTimer () {
      timeDisplayContent.value = '--:--:---'
      clearInterval(timer.value)
      remainingTimeInMilliseconds.value = props.timeInSeconds * 1000
    }

    /**
     * creates a formatted time string for the remaining time
     * @param milliseconds
     * @returns {string}
     */
    function getTimeDisplayContent (milliseconds) {
      const minutes = Math.floor(milliseconds / 60000)
      milliseconds = milliseconds - (minutes * 60000)
      const seconds = Math.floor(milliseconds / 1000)
      milliseconds = milliseconds - (seconds * 1000)

      return (minutes < 10 ? '0' : '') + minutes + ':' + (seconds < 10 ? '0' : '') + seconds + ':' + (milliseconds < 100 ? '0' : '') + (milliseconds < 10 ? '0' : '') + milliseconds
    }

    return {
      timeDisplayContent,
      startTimer,
      resetTimer
    }
  }
}
</script>

<style scoped>
.gametime {
  border-radius: 25px;
  font-size: 24px;
  width: 150px;
  margin: 8px;
  padding: 8px;
  color: aliceblue;
  background-color: forestgreen;
}
</style>