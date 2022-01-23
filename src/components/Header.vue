<template>
  <div class="header fixed">{{ title }}</div>
</template>

<script>
export default {
  name: "Header",
  props: {
    title: {
      type: String,
      default: () => { return 'Vue 3 Project' }
    }
  }
}
</script>

<style scoped>
.fixed {
  position: fixed;
  top: 0;
}

.header {
  width: 100%;
  padding: 8px 8px;
  font-size: 32px;
  color: aliceblue;
  background-color: #301515;
}

</style>