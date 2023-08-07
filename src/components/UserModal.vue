<template lang="">
  <div class="center">
        <Button label="Cargar Usuario" icon="pi pi-user" @click="openModal" />
        <Dialog v-model:visible="visible" @hide="closeModal" modal header="Ingrese usuario" :style="{ width: '50vw' }" :breakpoints="{ '960px': '75vw', '641px': '100vw' }">
            <InputText type="text" v-model="username" />
            <Button label="Guardar" @click="loadUser"/>
        </Dialog>
    </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import Card from "primevue/card";
import Button from "primevue/button";
import InputText from "primevue/inputtext";
import Dialog from "primevue/dialog";

const emit = defineEmits(["customChange"]);

const visible = ref(false);
const username = ref(null);

const openModal = () => {
  visible.value = true;
  username.value = null;
};

const loadUser = () => {
  if (username.value) {
    emit("loadUser", username.value);
    visible.value = false;
  }
};

const closeModal = () => {
  visible.value = false;
};
</script>
<style scoped>
.p-button {
  width: 100%;
}
.p-inputtext {
  width: 100%;
}
</style>