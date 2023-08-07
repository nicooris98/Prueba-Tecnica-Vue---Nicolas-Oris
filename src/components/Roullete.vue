<template lang="">
<Card>
    <template #title> Resultado </template>
    <template #content>
  <div class="center">
    <div class="result-info">
      <div :class="props.winnerClass">
        {{
          props.winnerClass == "winner"
            ? "Has Ganado"
            : props.winnerClass == "loser"
            ? "Has Perdido"
            : "Apuesta: "
        }}
        ${{ props.quantity }}
      </div>
      <div>
        <p><b>Número: </b>{{ props.number }}</p>
        <p :class="props.color">
          <b>Color: </b>{{ props.color == "r" ? "Rojo" : props.color == "b" ? "Negro" : "" }}
        </p>
        <p>{{ props.isEven == "e" ? "Es par" : props.isEven == "o" ? "Es impar" : "" }}</p>
      </div>
    </div>
    <img src="wheel-mobile.png" alt="Image" :class="'rotate ' + props.rotate" />
  </div>
</template>
  </Card>
</template>
<script setup lang="ts">
import Card from "primevue/card";
const props = defineProps([
  "color",
  "number",
  "winnerClass",
  "isEven",
  "quantity",
  "rotate",
]);

const emit = defineEmits(["customChange"]);

const closeModal = () => {
  console.log("close");
  emit("resetRoullete");
};
</script>
<style scoped>
.result-info {
  border: 1px solid black;
  border-radius: 5px;
  padding: 5px;
  box-shadow: 10px 5px 5px black;
}

.rotate {
  transform: rotate(0deg);
  width: 175px;
}

.true {
  transform: rotate(1080deg);
  width: 175px;
  transition: 3s;
}

.center {
  padding: 10px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  flex-direction: row;
}

.b {
  color: black;
}

.r {
  color: red;
}

.winner {
  color: green;
}

.loser {
  color: red;
}

.p-card {
  height: 30vh;
}
</style>