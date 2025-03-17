<template>
  <v-container class="alert-container">
    <v-alert :color="color" :icon="icon"> {{ text }}</v-alert>
  </v-container>
</template>
  
  
<script setup>
import { defineProps } from "vue";

const props = defineProps({
  text: String,
  color: String,
  icon: String,
});

console.log("Props recebidas:", props);
</script>
 <style>

.alert-container {
  position: fixed;

  bottom: 40px; /* Ajuste para ficar logo acima do footer */
  left: 0%;
  z-index: 1000;
  width: 16%;
  max-width: 16%;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2); /* Suaviza a aparência */
}
</style>