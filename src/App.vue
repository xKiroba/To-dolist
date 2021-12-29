<template>
  <v-app>
    <div>
      <v-app-bar app class="black" dark>

      <v-toolbar-title>Things to do </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-app-bar>


    </div>
    <v-main>
  </v-main>

      <router-view/>

  </v-app>
</template>

<script>
  export default {
    data: () => ({
      drawer: false,
      group: null,
    }),
  }
</script>
