<template>
  <q-item
    clickable
    :to="props.link"
  >
    <q-item-section>
      <q-item-label align="right">{{ props.title }}</q-item-label>
    </q-item-section>
    <q-item-section v-if="props.icon" avatar>
      <q-icon :name="props.icon" color="teal-10"/>
    </q-item-section>
  </q-item>
</template>

<script setup>
const props = defineProps({
  title: {
    type: String,
    required: true
  },
  link: {
    type: String,
    default: ''
  },
  icon: {
    type: String,
    default: '',
  }
})
</script>
