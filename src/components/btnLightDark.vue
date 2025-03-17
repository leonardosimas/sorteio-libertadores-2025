<script setup>
import { ref } from "vue";
import { useTheme } from "vuetify";

// Pega o tema atual do Vuetify
const theme = useTheme();

// Estado para controlar o tema
const isDarkMode = ref(false);

// Alterna entre os temas claro e escuro
const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value;
  theme.global.name.value = isDarkMode.value ? "light" : "dark";
};
</script>

<template>
  <v-list>
    <!-- Botão para alternar o tema -->
    <v-list-item @click="toggleTheme">
      <v-icon>{{
        isDarkMode ? "mdi-weather-night" : "mdi-weather-sunny"
      }}</v-icon>
      <!-- <v-list-item-title>
        {{ isDarkMode ? "Modo Claro" : "Modo Escuro" }}
      </v-list-item-title> -->
    </v-list-item>
  </v-list>
</template>
