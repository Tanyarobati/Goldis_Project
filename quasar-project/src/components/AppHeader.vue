<template>
  <q-header >
    <q-toolbar style="background-color: #f0f0f0;">
      <q-btn
        outline
        color="teal-10"
        label="سبد خرید من"
        icon="shopping_cart"
        style="margin-right: 10px;"
      />

      <q-btn-dropdown class="flat" color="teal-10" label="ورود | خروج">
        <div class="row no-wrap q-pa-md">
          <div class="column">
            <div class="text-h6 q-mb-md" align="center">{{ user.name }}</div>
            <q-btn
            glossy
            outline
            icon="login"
            v-model="login"
            color="teal-10"
            label="ورود به حساب "
            style="margin-bottom: 8px;"
            @click="routerLogin"
             />
            <q-btn
              glossy
              color="teal-10"
              outline
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
        <q-space/>
         <a class="navbar-brand" href="/">
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
import { ref } from 'vue'
import { api } from 'boot/axios'
import { useRouter } from 'vue-router'

const router = useRouter()

const user = ref('')
defineEmits(['toggleMenu'])

async function fetchUser () {
  const { data } = await api.get('/user')
  user.value = data[0]
}
async function deleteProfile () {
  await api.delete('/user/{{user.id}}')
}
function routerLogin () {
  router.push('/login')
}
fetchUser()
</script>

