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

      <transition name="fade" mode="out-in">
        <router-view></router-view
      ></transition>
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: 'App',

  methods: {}
}
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
