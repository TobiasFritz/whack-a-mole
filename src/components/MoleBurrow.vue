<template>
 <div
     v-show="active"
     class="burrow"
 >
   <b-img
       v-show="hasMole"
       class="mole"
       :src="require('@/assets/alien.svg')"
       @click="moleHit"
   />
 </div>
</template>

<script>
import { ref, watch } from "vue";

export default {
  name: "MoleBurrow",
  props: {
    active: {
      type: Boolean,
      default: true
    },
    hasMole: {
      type: Boolean,
      default: false
    },
    timeout: {
      type: Number,
      default: 3000
    }
  },
  emits: ['hit', 'update:hasMole'],
  setup (props, { emit }) {
    const timer = ref(null)

    /**
     * reset burrow state when time to hit the mole runs out
     */
    watch(() => props.hasMole, () => {
      timer.value = setTimeout(reset, props.timeout)
    })

    /**
     * reset burrow state
     */
    function reset () {
      emit('update:hasMole', false)
      clearTimeout(timer.value)
    }

    /**
     * reset burrow state and emit mole hit event used to reward the player with points
     */
    function moleHit () {
      if (props.active && props.hasMole) {
        emit('hit')
        reset()
      }
    }

    return  {
      moleHit
    }
  }
}
</script>

<style scoped>
.burrow {
  border-radius: 50px;
  width: 64px;
  height: 64px;
  margin: auto;
  border: 4px solid saddlebrown;
  background-color: black;
}

.mole {
  width: 48px;
  height: 48px;

  -webkit-animation: fadein 3s; /* Safari, Chrome and Opera > 12.1 */
  -moz-animation: fadein 3s; /* Firefox < 16 */
  -ms-animation: fadein 3s; /* Internet Explorer */
  -o-animation: fadein 3s; /* Opera < 12.1 */
  animation: fadein 3s;
}

@keyframes fadein {
  from { opacity: 0; }
  to   { opacity: 1; }
}

/* Firefox < 16 */
@-moz-keyframes fadein {
  from { opacity: 0; }
  to   { opacity: 1; }
}

/* Safari, Chrome and Opera > 12.1 */
@-webkit-keyframes fadein {
  from { opacity: 0; }
  to   { opacity: 1; }
}

/* Internet Explorer */
@-ms-keyframes fadein {
  from { opacity: 0; }
  to   { opacity: 1; }
}

/* Opera < 12.1 */
@-o-keyframes fadein {
  from { opacity: 0; }
  to   { opacity: 1; }
}

</style>