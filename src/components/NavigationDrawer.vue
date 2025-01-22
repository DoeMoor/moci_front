<template>
  <v-navigation-drawer
    permanent
    rail
    expand-on-hover
  >
    <v-list
      selectable
      item-props
      :items="items"
      nav
    />
  
    <template #append>
      <v-list>
        <v-list-item
          class="ma-2"
          link
          nav
          prepend-icon="mdi-cog-outline"
          title="Settings"
        />
        <v-list-item
          class="ma-2"
          nav
          @click="toggleTheme"
        >
          <template #prepend>
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

// const drawer = ref(true)

const items = ref([
  {
    title: 'Controller',
    // prependIcon: 'mdi-land-rows-vertical',
    prependIcon: 'mdi-raspberry-pi',
    link: true,
    to: '/controller',
  },
  {
    title: 'Io-modules',
    prependIcon: 'mdi-expansion-card',
    link: true,
    to: '/io-modules',
  },
  {
    title: 'Customer',
    prependIcon: 'mdi-briefcase-outline',
    link: true,
    to: '/customer',
  },
  // {
  //   title: 'Manufacturer',
  //   prependIcon: 'mdi-factory',
  //   link: true,
  //   to: '/',
  // },
])
</script>