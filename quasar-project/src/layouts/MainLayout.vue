<template>
  <q-layout view="lHh Lpr lFf">
    <app-header @toggle-menu="onToggleMenu"/>
    <app-drawer v-model="rightDrawerOpen"/>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { ref } from 'vue'

import AppHeader from 'src/components/AppHeader.vue'
import AppDrawer from 'src/components/AppDrawer.vue'

const rightDrawerOpen = ref(false)

function onToggleMenu () {
  rightDrawerOpen.value = !rightDrawerOpen.value
}
</script>
