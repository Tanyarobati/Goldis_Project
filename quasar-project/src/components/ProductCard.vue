<template>
  <q-card class="product-card" style="background-color: #f0f0f0;">

        <q-img
          :src="product.image"
          ratio="4:3"
          alt="Product Image"
        />
        <img
        :src="product.image"
        class="card-image"
        >

    <q-card-section>
      <div class="text-h7 text-bold" align="right">{{ product.name }}</div>
      <div class="text-subtitle2 text-grey" align="right">{{ product.sold ? 'ناموجود' : 'موجود' }}</div>
    </q-card-section>

    <q-card-section align="right">
      <div>
        <span>قیمت:</span> ${{ product.price }}
      </div>
      <q-rating :value="product.rate" size="18px" color="yellow" />
    </q-card-section>

    <q-card-actions align="around">
      <q-btn v-if="actions.delete" label="حذف" icon="delete" outlined style="color: red;" @click="deleteProduct" />
      <q-btn v-if="actions.edit" label="ویرایش" icon="edit" color="teal-10" @click="editProduct" />
      <q-btn v-if="!actions.edit && !actions.delete" label="جزئیات" flat />
      <q-btn v-if="!actions.edit && !actions.delete" label="افزودن به سبد خرید" color="teal-10" icon="shopping_cart" />
    </q-card-actions>
  </q-card>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  product: {
    type: Object,
    required: true
  },
  actions: {
    type: Object,
    default: () => ({
      edit: false,
      delete: false
    })
  }
})

const emit = defineEmits(['edit', 'delete'])

function editProduct() {
  emit('edit', props.product)
}

function deleteProduct() {
  emit('delete', props.product.id)
}
</script>

<style scoped>
.product-card {
  width:250px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}
.card-image {
  border-top-right-radius: 10px;
  border-top-left-radius: 12px;
  width: 250px;
  height: 180px;
  margin-bottom: 5px;
  margin-top: -20px;

}
</style>
