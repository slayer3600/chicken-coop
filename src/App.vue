<template>
  <v-app>
    <v-main>
      <ChickenCoop />
    </v-main>
  </v-app>
</template>

<script>
import ChickenCoop from './components/ChickenCoopTemp.vue';

export default {
  name: 'App',

  components: {
    ChickenCoop,
  },

  data: () => ({
    //
  }),
};
</script>
