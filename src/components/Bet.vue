<template lang="">
  <InputNumber :disabled="props.username == null" v-model="bet" inputId="currency-us" mode="currency" currency="USD" locale="en-US" />
  <TabBet
    @change-color-number="changeColorNumber"
    @change-number="changeNumber"
    @change-even="changeEven"
    @change-color="changeColor"
    @reset-bet-type="resetBetType"
  />
  <Button icon="pi pi-play" :disabled="props.username == null" label="Jugar" severity="info" @click="play"/>
  <Button icon="pi pi-save" :disabled="props.saved" label="Guardar" severity="success" @click="updateUser"/>
  <Roullete
    @reset-roullete="resetRoullete"
    :isEven="resultEven"
    :winnerClass="winnerClass"
    :number="resultNumber"
    :quantity="bet"
    :rotate="rotate"
    :color="resultColor"
  />
</template>
<script setup lang="ts">
import Button from "primevue/button";
import InputNumber from "primevue/inputnumber";
import { ref } from "vue";
import confetti from "canvas-confetti";
import TabBet from "./TabBet.vue";
import Roullete from "./Roullete.vue";

const props = defineProps(["saved", "amount", "username"]);

const emit = defineEmits(["customChange"]);

const bet = ref(0);

const rotate = ref("false");
const winnerClass = ref(null);

const selectedColor = ref(null);

let selectedEven: boolean = null;

const resultEven = ref("");
const resultNumber = ref(null);
const resultColor = ref(null);

const selectedNumber = ref(null);

const resetRoullete = () => {
  resultEven.value = ""
  resultColor.value = null
  resultNumber.value = null
  rotate.value = "false"
  bet.value = 0
}

const changeColorNumber = (newColor: string) => {
  selectedColor.value = newColor;
};

const changeNumber = (newNumber: number) => {
  selectedNumber.value = newNumber;
};

const changeEven = (newEven: boolean) => {
  selectedEven = newEven;
};

const changeColor = (newColor: string) => {
  selectedColor.value = newColor;
};

const resetBetType = () => {
  selectedColor.value = null;
  selectedEven = null;
  selectedColor.value = null;
  selectedNumber.value = null;
};

const getNumber = async () => {
  try {
    const numberApi = await fetch(`${import.meta.env.VITE_API_URL}/api/Number`);
    return await numberApi.json();
  } catch (error) {
    console.error(error);
  }
};

const getColor = async () => {
  try {
    const colorApi = await fetch(`${import.meta.env.VITE_API_URL}/api/Color`);
    return await colorApi.json();
  } catch (error) {
    console.error(error);
  }
};

const isEven = (numberValue: number): boolean => {
  return numberValue % 2 == 0;
};

const updateUser = async () => {
  try {
    const requestOptions = {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: `${props.amount}`,
    };
    const userData = await (
      await fetch(
        `${import.meta.env.VITE_API_URL}/api/User/?username=${props.username}`,
        requestOptions
      )
    ).json();
    emit("setAmount", userData.amount);
    emit("setSave", true);
  } catch (error) {
    console.log(error);
  }
};

const play = async () => {
  if (
    props.username == "" ||
    props.username == null ||
    props.username == undefined
  )
    return;

  if (bet.value == 0 || bet.value == null || bet.value == undefined) return;

  if (
    selectedColor.value == null &&
    selectedEven == null &&
    (selectedColor.value == null || selectedNumber.value == null)
  )
    return;
  rotate.value = "false";
  const numberRoullete: number = await getNumber();
  const colorRoullete: string = await getColor();
  const evenNumber: boolean = isEven(numberRoullete);
  emit("setSave", false);

  setTimeout(() => {
    rotate.value = "true";
  }, 0.1);

  setTimeout(() => {
    resultEven.value = evenNumber ? "e" : "o";
    resultColor.value = colorRoullete
    resultNumber.value = numberRoullete
    if (selectedColor.value && selectedColor.value == colorRoullete) {
      confetti();
      const newAmount = props.amount + bet.value / 2;
      emit("setAmount", newAmount);
      winnerClass.value = "winner";
      return;
    }

    if (
      selectedEven !== null &&
      (selectedEven == evenNumber || !selectedEven == !evenNumber)
    ) {
      confetti();
      const newAmount = props.amount + bet.value;
      emit("setAmount", newAmount);
      winnerClass.value = "winner";
      return;
    }

    if (
      selectedColor.value &&
      selectedNumber.value &&
      selectedColor.value == colorRoullete &&
      selectedNumber.value == numberRoullete
    ) {
      confetti();
      const newAmount = props.amount + bet.value * 3;
      emit("setAmount", newAmount);
      winnerClass.value = "winner";
      return;
    }

    const newAmount = props.amount - bet.value;
    emit("setAmount", newAmount);
    winnerClass.value = "loser";
  }, 3000);
};
</script>
<style scoped>
.p-button {
  width: 100%;
}
.p-inputnumber {
  width: 100%;
}
</style>