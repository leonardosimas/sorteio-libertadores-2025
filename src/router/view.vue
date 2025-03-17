<template>
    <v-app id="inspire">
      <v-navigation-drawer v-model="drawer">
        <!-- Criando as primeiras rotas de teste -->
        <nav>
          <v-img
            src="https://picsum.photos/1920/1080?ramdom"
            gradient="to top right, rgba(19,84,122,.8), rgba(128,208,199,.8)"
            height="80"
            cover
            class="pt-2"
          >
            <v-list>
              <v-list-item
                prepend-avatar="https://github.com/yurimarcon/avatars/blob/main/Colored/ToyFaces_Colored_BG_29.jpg?raw=true"
                subtitle="Lista de Tarefas"
                title="Leonardo"
              >
              </v-list-item>
            </v-list>
          </v-img>
  
          <v-divider></v-divider>
  
          <v-list :lines="false" density="compact" nav>
            <RouterLink
              v-for="(item, i) in items"
              :key="i"
              :to="item.to"
              style="text-decoration: none; color: inherit"
            >
              <v-list-item :value="item" color="primary">
                <template v-slot:prepend>
                  <v-icon :icon="item.icon"></v-icon>
                </template>
                <v-list-item-title :textContent="item.text"></v-list-item-title>
              </v-list-item>
            </RouterLink>
          </v-list>
        </nav>
      </v-navigation-drawer>
  
      <v-app-bar
        color="teal-darken-4"
        image="https://picsum.photos/1920/1080?ramdom"
        density="prominent"
      >
        <template v-slot:image>
          <v-img
            gradient="to top right, rgba(19,84,122,.8), rgba(128,208,199,.8)"
          ></v-img>
        </template>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
        <v-app-bar-title>Lista de Tarefas</v-app-bar-title>
      </v-app-bar>
  
      <v-main>
        <router-view></router-view>
      </v-main>
    </v-app>
  </template>
    
    <script setup>
  import { ref } from "vue";
  
  const drawer = ref(null);
  
  const items = [
    { text: "Home", icon: "mdi-home", to: "/Home" },
    { text: "About", icon: "mdi-information", to: "/About" },
  ];
  </script>
  
  <style scoped>
  /* Aplica estilo ao elemento <a> gerado por <RouterLink> */
  a {
    text-decoration: none;
    color: #505050;
  }
  </style>
    