<template>
  <v-navigation-drawer permanent rail expand-on-hover>
    <v-list selectable item-props :items="items" nav />
  
    <template #append>
      <v-list>
        <v-list-item class="ma-2" link nav prepend-icon="mdi-cog-outline" title="Settings" />
        <v-list-item @click="toggleTheme" class="ma-2" nav>
          <template v-slot:prepend>
            <v-icon :icon="isDark ? 'mdi-weather-sunny' : 'mdi-weather-night'" />
          </template>
          <v-list-item-title>{{ isDark ? 'Light Mode' : 'Dark Mode' }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </template>
  </v-navigation-drawer>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useTheme } from 'vuetify'

const theme = useTheme()

const isDark = computed(() => theme.global.current.value.dark)

const toggleTheme = () => {
  theme.global.name.value = isDark.value ? 'light' : 'dark'
}

const drawer = ref(true)

const items = ref([
  {
    title: 'moduline',
    prependIcon: 'mdi-view-dashboard-outline',
    link: true,
    to: '/moduline',
  },
  {
    title: 'io-modules',
    prependIcon: 'mdi-account-group',
    link: true,
    to: '/io-modules',
  },
  {
    title: 'customer',
    prependIcon: 'mdi-briefcase-outline',
    link: true,
    to: '/customer',
  },
])
</script>