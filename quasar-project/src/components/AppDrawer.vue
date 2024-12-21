<template>
  <q-drawer style="background-color: #f0f0f0;"
      v-model="modelValue"
      bordered
      side="right"
    >
      <q-list>
        <EssentialLink
          v-for="(link , index) in linksList"
          :key="index"
          v-bind="link"
        />
      </q-list>
    </q-drawer>
</template>
<script setup>
import EssentialLink from './EssentialLink.vue'

const modelValue = defineModel({ default: false })

const linksList = [
  {
    title: 'مدیریت محصولات',
    icon: 'manage_accounts',
    link: '/users'
  },
  {
    title: ' درباره ما',
    icon: 'info',
    link: '/users'
  }

]
</script>
