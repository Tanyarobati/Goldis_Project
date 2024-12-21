<template>
  <q-dialog
    v-model="localVisible"
  >
    <q-card style="max-width: 400px">
      <q-card-section>
        <div class="text-h6" align="center">{{ isNewProduct ? 'افزودن محصول' : 'ویرایش محصول' }}</div>
      </q-card-section>

      <q-card-section >
        <q-form @submit.prevent="submit">
          <q-input
            v-model="formData.name"
            label="نام محصول"
            outlined
            class="q-mb-md"
          />
          <q-input
            v-model="formData.price"
            label="قیمت محصول"
            outlined
            class="q-mb-md"
          />
          <q-input
            v-model="formData.description"
            label="توضیحات محصول"
            type="textarea"
            outlined
            class="q-mb-md"
          />
          <q-input
            v-model="formData.image"
            label="تصویر محصول"
            outlined
            class="q-mb-md"
            type="url"
            accept="image/*"
          />

          <q-toggle
            color="teal-10"
            false-value="موجود"
            true-value="ناموجود"
            v-model="formData.sold"
            class="q-mb-md"
            :label="formData.sold"
          />
          <div>
            <q-btn
              type="submit"
              label="ذخیره"
              color="teal-10"
            />
            <q-btn
              flat
              label="لغو"
              color="negative"
              @click="close"
            />
          </div>
        </q-form>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  product: {
    type: Object,
    required: true
  },
  visible: {
    type: Boolean,
    required: true
  },
  isNewProduct: {
    type: Boolean,
    default: false
  }
})

const emits = defineEmits(['update:visible', 'onSubmit'])

const localVisible = ref(props.visible)
const formData = ref({ ...props.product, image: null })

watch(
  () => props.visible,
  (newVisible) => {
    localVisible.value = newVisible
  }
)

watch(
  () => localVisible.value,
  (newVisible) => {
    emits('update:visible', newVisible)
  }
)

watch(
  () => props.product,
  (newProduct) => {
    formData.value = { ...newProduct, image: null }
  },
  { immediate: true }
)

function close() {
  localVisible.value = false
}

function submit() {
  emits('onSubmit', formData.value)
  close()
}
</script>
