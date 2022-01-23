<template>
  <div class="footer fixed"/>
</template>

<script>
export default {
  name: "Footer"
}
</script>

<style scoped>
.fixed {
  position: fixed;
  bottom: 0;
}

.footer {
  width: 100%;
  height: 32px;
  padding: 8px 8px;
  font-size: 32px;
  color: aliceblue;
  background-color: #301515;
}

</style>