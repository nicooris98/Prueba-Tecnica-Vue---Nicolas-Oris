<script setup lang="ts">
import Board from './components/Board.vue'
</script>

<template>
  <div class="container-center">
    <Board />
  </div>
</template>

<style scoped>
.container-center {
  background-color: rgb(208, 215, 221);
  margin: auto;
  width: 50%;
  border-radius: 5px;
  padding: 10px;
  height: 95vh;
  margin-top: 5px;
}
</style>
