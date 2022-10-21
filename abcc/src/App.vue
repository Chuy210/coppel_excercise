<template>
  <v-app>
    <v-main>
      <Abcc/>
    </v-main>
  </v-app>
</template>

<script>
import Abcc from './components/Abcc.vue';

export default {
  name: 'App',

  components: {
    Abcc,
  },

  data: () => ({
    //
  }),
};
</script>
