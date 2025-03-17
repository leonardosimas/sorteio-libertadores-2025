<template>
  <v-col cols="12" md="6" lg="3">
    <v-card class="grupo-card" :elevation="4" rounded="lg">
      <v-card-title class="card-title">{{ grupo }}</v-card-title>
      <v-divider />
      <v-card-text>
        <ul class="grupo-lista">
          <li class="span" v-for="(time, index) in times" :key="index">
            {{ time.Nome }} - {{ time.Pais }}
          </li>
        </ul>
      </v-card-text>
    </v-card>
  </v-col>
</template>
  
  <script setup>
import { defineProps } from "vue";

const props = defineProps({
  grupo: String,
  times: Array,
});
</script>
  
  <style scoped>
.grupo-card {
  text-align: center;
  margin-bottom: 16px;
  text-transform: uppercase;
}

.card-title {
  text-transform: uppercase;
  font-weight: bold;
}
.grupo-lista {
  list-style-type: none;
  padding: 0;
  font-size: 14px;
  font-weight: bold;
  text-transform: uppercase;
  flex-grow: 1;
  width: 100%;
  text-align: center;
}

.grupo-lista li {
  padding: 4px 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}
</style>
  