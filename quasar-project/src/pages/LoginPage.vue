<template>
  <q-page class="flex flex-center">
  <div class="q-pa-md">

    <q-form
      class="q-gutter-md"
      v-model="myform"
      @submit.prevent="submit"
    >
      <q-input
        filled
        class="field_label"
        v-model="myform.name"
        label="نام کاربری"
        lazy-rules
        :rules="[requiredRule]"
      />
      <q-input
        filled
        v-model="myform.email"
        label="ایمیل"
        lazy-rules
        :rules="[emailRule]"
      />

      <q-input
        filled
        v-model="myform.password"
        label="گذرواژه"
        lazy-rules
        :rules="[requiredRule]"
      />

      <div >
        <q-btn label="ذخیره" type="submit" color="teal-10" class="full-width" style="margin-top: -10px;"/>
      </div>
    </q-form>

  </div>
</q-page>
</template>

<script setup>
import { useQuasar } from 'quasar'
import { emailRule, requiredRule } from 'src/utils/rules';
import { reactive } from 'vue'
import { useRouter } from 'vue-router'

const $q = useQuasar()
const router = useRouter()

const myform = reactive({
  name: null,
  password: null,
  email: null
})

function submit () {
  $q.notify({
    color: 'teal-10',
    message: `${myform.name} با موفقیت وارد شد `
  })
  router.push('/')
}
</script>
<style>
.field_label {
  text-align: right;
}
</style>
