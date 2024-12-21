<template>
  <q-header >
    <q-toolbar style="background-color: #f0f0f0;">
      <q-btn
        outline
        color="teal-10"
        label="سبد خرید من"
        icon="shopping_cart"
      />

      <q-btn-dropdown class="flat" color="teal-10" label="ورود | خروج">
        <div class="row no-wrap q-pa-md">
          <div class="column">
            <div class="text-h6 q-mb-md" align="center">{{ user.name }}</div>
            <q-btn
            glossy
            color="orange-4"
            v-model="login"
            label="ورود به حساب "
            icon="login" />
            <q-btn
            glossy
            color="orange-4"
              v-model="logout"
              label="خروج از حساب "
              icon="logout"
              @click="deleteProfile"
            />
          </div>

          <q-separator vertical inset class="q-mx-lg" />

          <div class="column items-center">
            <q-avatar size="72px">
              <img :src="user.avatar" />
            </q-avatar>
          </div>
        </div>
      </q-btn-dropdown>
        <q-space></q-space>
        <a class="navbar-brand" >
                    <img class="text-logo" src="src/assets/goldis-logo-horizontal.png" width="140">
                </a>
        <q-btn
         icon="list"
         flat color="teal-10"
         size="20px"
         @click="$emit('toggleMenu')"></q-btn>
    </q-toolbar>
  </q-header>

</template>

<script setup>
import { api } from 'boot/axios'
import { ref } from 'vue'

const user = ref('')
defineEmits(['toggleMenu'])

async function fetchUser () {
  const { data } = await api.get('/user')
  user.value = data[0]
}
async function deleteProfile () {
  await api.delete('/user/{{user.id}}')
}
fetchUser()
</script>

