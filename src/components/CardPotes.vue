<template>
    <v-card class="pote-card" :elevation="4" rounded="lg">
      <v-card-title class="pote-title">
        {{ pote }}
      </v-card-title>
      <v-divider />
  
      <v-card-text class="d-flex flex-column align-center">
        <ul class="pote-lista">
          <li v-for="(time, index) in times" :key="index">
            {{ time.Nome }}
          </li>
        </ul>
      </v-card-text>
      <v-card-actions class="justify-center">
        <v-divider />
        <v-btn :disabled="isDisabled" @click="sortearPote" color="primary">
          Sortear {{ pote }}
        </v-btn>
      </v-card-actions>
    </v-card>
  </template>
  
  <script setup>
  import { defineProps, computed } from "vue";
  
  const props = defineProps({
    pote: String,
    times: Array,
    disabled: Boolean,
    sortearPote: Function,
  });
  
  // Verifica se o valor de 'disabled' é realmente um Booleano
  const isDisabled = computed(() => {
    console.log('Valor de props.disabled:', props.disabled);
    console.log('Tipo de props.disabled:', typeof props.disabled);
    return typeof props.disabled === 'boolean' ? props.disabled : false;
  });
  
  console.log("Props no CardPotes", props);
  </script>
  
  <style scoped>
  .pote-card {
    padding: 16px;
    text-align: center;
    font-family: Verdana, sans-serif;
    width: 100%;
    min-height: 350px;
    max-width: 400px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
  }
  
  .pote-title {
    font-size: 1.2rem;
    font-weight: bold;
    text-transform: uppercase;
  }
  
  .pote-lista {
    list-style-type: none;
    padding: 0;
    font-size: 14px;
    font-weight: bold;
    text-transform: uppercase;
    flex-grow: 1;
    width: 100%;
    text-align: center;
  }
  
  .pote-lista li {
    padding: 4px 0;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 100%;
  }
  
  .v-btn {
    background-color: #5a4024 !important;
    color: #ffffff !important;
    margin-top: auto;
    font-weight: bold;
    text-transform: uppercase;
  }
  </style>
  