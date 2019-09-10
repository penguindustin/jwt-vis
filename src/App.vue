<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>JWT</span>
        <span class="font-weight-light ml-3">A guide</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text href="" target="_blank">
        <span class="mr-2">By Dustin Mendoza</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <JWT />
    </v-content>
  </v-app>
</template>

<script>
import JWT from "./components/JWT";

export default {
  name: "App",
  components: {
    JWT
  },
  data: () => ({
    //
  })
};
</script>
