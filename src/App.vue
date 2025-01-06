<template>
  <v-app>
    <NavigationDrawer />
    <v-main :class="{ 'bg-background': true }">
      <router-view />
    </v-main>
  </v-app>
</template>

<script setup>
//
</script>
