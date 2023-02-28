<template>
  <div class="flex min-h-screen flex-col bg-weather-primary font-Roboto">
    <SiteNavigation />
    <RouterView />
  </div>
</template>

<script setup>
import SiteNavigation from './components/SiteNavigation.vue'
import { RouterView } from 'vue-router'
</script>

<style lang="scss" scoped></style>
