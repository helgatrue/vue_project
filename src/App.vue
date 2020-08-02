<template v-slot:header>
  <v-app>
    <v-app-bar
      app
      color="green"
      dark
    >
    </v-app-bar>
        <v-main>
      <Table/>
    </v-main>
  </v-app>
</template>

<script>
import Table from './components/Table';

export default {
  name: 'App',

  components: {
    Table
  },

  data: () => ({
    //
  }),
};
</script>
