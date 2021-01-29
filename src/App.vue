<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <h2 class="shrink mr-2" transition="scale-transition">
          Train Travel Web
        </h2>
      </div>
    </v-app-bar>
    <v-main>
      <home-view />
    </v-main>
  </v-app>
</template>

<script>
import HomeView from "./components/Home.View.vue";

export default {
  name: "App",
  components: {
    HomeView,
  },
};
</script>
