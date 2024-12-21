<template>
  <q-page>
      <div class="q-pa-md q-pt-xl">
        <q-row
          class="q-gutter-md justify-evenly"
          :cols="4"
          style="display: flex; flex-wrap: wrap;"
        >

        <q-col
            v-for="product in products"
            :key="product.id"
            :cols="12" sm="6" md="3" lg="3"
            class="product-col"
          >

          <ProductCard
            :product="product"
            :actions="{ edit: true, delete: true }"
            @edit="editProduct"
            @delete="deleteProduct"
          />
        </q-col>
      </q-row>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import ProductCard from 'components/ProductCard.vue'
import { api } from 'boot/axios'

const products = ref([])

async function fetchProducts() {
  const { data } = await api.get('/products')
  products.value = data
}
fetchProducts()

function editProduct(product) {
  console.log('Editing product:', product)
}

async function deleteProduct(id) {
  try {
    await api.delete(`/products/${id}`)
    fetchProducts()
  } catch (error) {
    console.error('Error deleting product:', error)
  }


}
</script>
