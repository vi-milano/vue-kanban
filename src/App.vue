<template>
  <v-app>
    <v-content>
      <v-app-bar app dark color="primary" dense>
        <router-link to="/"> <v-icon>mdi-view-week</v-icon></router-link>

        <v-spacer />
        <v-toolbar-title style="cursor: default">Vue Kanban</v-toolbar-title>
        <v-spacer />
        <router-link to="/about"><v-icon>mdi-help</v-icon></router-link>
      </v-app-bar>
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: 'App',

  methods: {}
}
</script>

<style></style>
