<template>
  <v-app>
    <v-app-bar
      app
      color="#FFF"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Evenstad Logo"
          class="shrink mr-2"
          contain
          :src="require('./assets/logo_challenge.png')"
          transition="scale-transition"
          width="200"
        />
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <Battleground/>
    </v-main>
  </v-app>
</template>

<script>
import Battleground from './components/Battleground';

export default {
  name: 'App',

  components: {
    Battleground,
  },

  data: () => ({
    //
  }),
};
</script>
