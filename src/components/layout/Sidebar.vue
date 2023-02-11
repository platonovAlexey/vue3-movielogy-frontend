<script lang="ts" setup>
import { useRoute } from 'vue-router'
import { HomeIcon, FilmIcon, MagnifyingGlassIcon } from '@heroicons/vue/24/outline'
import { 
  HomeIcon as HomeIconSolid,
  FilmIcon as FilmSolidIcon,
  MagnifyingGlassIcon as MagnifyingGlassSolidIcon
 } from '@heroicons/vue/24/solid'
const menu = [
  {
    to: '/', icon: HomeIcon, activeIcon: HomeIconSolid
  },
  {
    to: '/search', icon: MagnifyingGlassIcon, activeIcon: MagnifyingGlassSolidIcon
  },
  {
    to: '/movies/1', icon: FilmIcon, activeIcon: FilmSolidIcon
  }
]

const route = useRoute()
</script>

<template>
  <nav class="block bg-movie-black-200">
    <div class="relative flex w-30 flex-col p-10 space-y-3">
      <RouterLink
        v-for="nav in menu"
        :to="nav.to"
        :key="nav.to"
        :class="[
          route.path === nav.to ? 'bg-movie-black-400 text-white' : 'text-gray-600 hover:bg-movie-black-400',
          'flex-shrink-0 inline-flex items-center justify-center h-14 w-14 rounded'
        ]"
      >
        <component :is="route.path === nav.to ? nav.activeIcon : nav.icon" class="h-6 w-6"></component>
      </RouterLink>
    </div>
  </nav>
</template>