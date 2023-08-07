<template lang="">
  <TabView @tab-change="onTabChange">
    <TabPanel header="Color">
        <Dropdown @change="changeColor" v-model="selectedColor" :options="colors" optionLabel="name" optionValue="code" placeholder="Selecciona un Color" class="w-full md:w-14rem" />
    </TabPanel>
    <TabPanel header="Par o Impar">
        <Dropdown @change="changeEven" v-model="selectedEven" :options="pares" optionLabel="name" optionValue="code" placeholder="Selecciona si es Par o Impar" class="w-full md:w-14rem" />
    </TabPanel>
    <TabPanel header="Número y Color">
        <InputNumber @input="changeNumber" v-model="selectedNumber" inputId="integeronly" :min="0" :max="36" />
        <Dropdown @change="changeColorNumber" v-model="selectedColorNumber" :options="colors" optionLabel="name" optionValue="code" placeholder="Selecciona un Color" class="w-full md:w-14rem" />
    </TabPanel>
  </TabView>
</template>
<script setup lang="ts">
import { ref } from "vue";
import TabView from "primevue/tabview";
import TabPanel from "primevue/tabpanel";
import Dropdown from "primevue/dropdown";
import InputNumber from "primevue/inputnumber";

const selectedColor = ref(null);

const selectedEven = ref(null);

const selectedNumber = ref(0);
const selectedColorNumber = ref(null);

const emit = defineEmits(["customChange"]);

const colors = ref([
  { name: "Negro", code: "b" },
  { name: "Rojo", code: "r" },
]);

const pares = ref([
  { name: "Par", code: true },
  { name: "Impar", code: false },
]);

const onTabChange = () => {
  emit("resetBetType");
};

const changeColorNumber = (event) => {
  emit("changeColorNumber", event.value);
}

const changeNumber = (event) => {
  if(event.value < 37) {
    emit("changeNumber", event.value);
  }
}

const changeColor = (event) => {
  emit("changeColor", event.value);
};

const changeEven = (event) => {
  emit("changeEven", event.value);
};
</script>
<style scoped>
</style>