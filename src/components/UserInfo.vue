<template lang="">
  <Card>
    <template #title> Datos del Usuario </template>
    <template #content>
  <div class="card-body">
      <p><b>Usuario:</b> {{ props.username }}</p>
      <p><b>Monto:</b> ${{ props.amount.toFixed(2) }}</p>
      <p v-if="!props.saved" class="p-warning">
        <i class="pi pi-info-circle"></i>Partida sin guardar.
      </p>
      <img :src="'https://robohash.org/'+props.username" alt="Image" class="img-user"/>
  </div>
</template>
  </Card>
</template>
<script setup lang="ts">
import Card from "primevue/card";
const props = defineProps(["username", "amount", "saved"]);
</script>
<style scoped>
.p-warning {
  color: orange;
}

.card-body {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.img-user {
  width: 150px;
  bottom: 100px;
  position: relative;
}

.p-card {
  height: 20vh;
}
</style>