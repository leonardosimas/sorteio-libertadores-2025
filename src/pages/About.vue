<template>
  <v-app-bar :elevation="2" rounded>
    <!-- <template v-slot:prepend>
      <v-app-bar-nav-icon> </v-app-bar-nav-icon>
    </template> -->

    <v-app-bar-title class="appbar-title" style="font-family: Ubuntu">
      Simulador para o sorteio Libertadores 2025
    </v-app-bar-title>
    <RouterLink to="/" style="text-decoration: none; color: inherit">
        <v-list-item :value="item" color="primary">
          <template v-slot:prepend>
            <v-icon icon="mdi-home"></v-icon>
          </template>
          <v-list-item-title :textContent="Home"></v-list-item-title>
        </v-list-item>
      </RouterLink>
    <BtnLightDark />
  </v-app-bar>
  <div>
    <div class="ma-4 text-center">
      <h1 class="ma-4">Obrigado!</h1>
      <p class="ma-4">Algumas tecnologias utilizadas no projeto.</p>
      <v-divider></v-divider>
    </div>
    <div></div>
    <div class="ma-4 d-flex">
      <AboutCard
        v-for="(item, index) in items"
        :key="index"
        :image="item.image"
        :title="item.title"
        :subtitle="item.subtitle"
        :text="item.text"
      />
    </div>

    <div class="text-center">
      <v-divider class="ma-4"></v-divider>
      <a
        target="_blank"
        href="https://github.com/leonardosimas"
        alt="Meu Github"
      >
        <v-btn color="primary"> 👨‍💻 Minhas mídias sociais 🤓 ✌️ </v-btn>
      </a>
    </div>
  </div>
</template>
    
    <script setup>
import { ref, onMounted } from "vue";
import AboutCard from "@/components/About/AboutCard.vue";

import "@/styles/Home.scss";
import "@fontsource/ubuntu";
import BtnLightDark from "@/components/btnLightDark.vue";

const items = [
  {
    image:
      "https://miro.medium.com/v2/resize:fit:1400/format:webp/1*OrjCKmou1jT4It5so5gvOA.jpeg",
    title: "Vue.js",
    subtitle: "The Progressive JavaScript Framework",
    text: "Vue (pronounced /vjuː/, like view) is a JavaScript framework for building user interfaces. It builds on top of standard HTML, CSS, and JavaScript and provides a declarative and component-based programming model that helps you efficiently develop user interfaces, be they simple or complex.",
  },
  {
    image:
      "https://cdn.vuetifyjs.com/docs/images/logos/vuetify-logo-dark-atom.svg",
    title: "Vuetify 3",
    subtitle: "Incível biblioteca front-end",
    text: "Learn more about what Vuetify is, how to create an application from scratch, browse API references, sample code, tutorials, and more.",
  },
  {
    image:
      "https://fineproxy.org/wp-content/uploads/2023/07/netlify.app_logo.png",
    title: "Netlify",
    subtitle: "Netlify Embeds Serverless Functionality into Its Web",
    text: "Netlify is the modern development platform for Enterprises to realize the speed, agility and performance of a scalable, composable web architecture.",
  },
  // {
  //   image:
  //     "https://miro.medium.com/v2/resize:fit:1400/format:webp/1*UWpmhI3AEdFY7gjHIZH-2w.png",
  //   title: "PWA",
  //   subtitle: "Progressive Web App",
  //   text: "Web apps have long played second-fiddle to native ones. While mobile apps benefit from full access to device APIs, snappy performance and support for features such as offline content and push notifications, the stateless and browser sandboxed nature of the web has frequently relegated web-based apps to second choice.",
  // },
];
</script>
    
    <style>
</style>
      