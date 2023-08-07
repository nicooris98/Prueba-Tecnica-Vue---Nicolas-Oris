<template lang="">
  <div class="container">
    <UserModal @load-user="loadUser"/>
    <UserInfo v-if="username" :username="username" :amount="amount" :saved="saved"/>
    <Bet @set-amount="setAmount" @set-save="setSave" :username="username" :amount="amount" :saved="saved"/>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import UserModal from "./UserModal.vue";
import UserInfo from "./UserInfo.vue";
import Bet from "./Bet.vue";

const amount = ref(0);
const username = ref(null);
const saved = ref(true);

const loadUser = async (usernameUrl) => {
  try {
    const userData = await (
      await fetch(`${import.meta.env.VITE_API_URL}/api/User/?username=${usernameUrl}`)
    ).json();
    username.value = userData.username;
    amount.value = userData.amount;
    saved.value = true;
  } catch (error) {
    console.error(error);
  }
};

const setSave = (newSave: boolean) => {
  saved.value = newSave
}

const setAmount = (newAmount: number) => {
  amount.value = newAmount
}
</script>
<style scoped>
  .container {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
</style>