<script setup lang="ts">
import Sidebar from './components/layout/Sidebar.vue';

</script>

<template>
  <div class="flex h-screen flex-col">
    <div class="flex flex-1 overflow-hidden">
      <Sidebar></Sidebar>
      <main class="min-w-0 flex flex-1">
        <RouterView />
      </main>
    </div>
  </div>
</template>
