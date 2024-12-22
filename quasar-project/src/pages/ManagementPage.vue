<template>
  <q-page>
      <div class="q-pa-md q-pt-xl">
        <q-btn
        label="افزودن محصول"
        color="teal-10"
        @click="openAddDialog"
        class="q-mb-md"
      />
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
            @edit="openEditDialog"
            @delete="deleteProduct"
          />
        </q-col>
      </q-row>
    </div>
    <EditProductDialog
      v-if="selectedProduct"
      v-model:visible="editDialog"
      :product="selectedProduct"
      @onSubmit="submitProduct"
      :isNewProduct="isNewProduct"
      />
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { api } from 'boot/axios'
import ProductCard from 'components/ProductCard.vue'
import EditProductDialog from 'src/components/EditProductDialog.vue'

const products = ref([])
const editDialog = ref(false)
const isNewProduct = ref(false)
const selectedProduct = ref(null)

async function fetchProducts() {
  const { data } = await api.get('/products')
  products.value = data
}
fetchProducts()

function openEditDialog(product) {
  selectedProduct.value = product
  editDialog.value = true
  isNewProduct.value = false
}
function openAddDialog() {
  selectedProduct.value = {}
  isNewProduct.value = true
  editDialog.value = true
}

async function submitProduct(product) {
  if (isNewProduct.value) {
    await addNewProduct(product)
  } else {

    await updateProduct(product)
  }
}


async function addNewProduct(newProduct) {
  try {
    const response = await api.post('/products', {
      name:newProduct.name || 'default',
      price: newProduct.price || '0.0',
      description: newProduct.description || 'no description',
      sold: newProduct.sold || true ,
      image: newProduct.image || 'https://i.postimg.cc/x1GYVD1p/goldis-logo-horizontal.png',
      rate: newProduct.rate || 5,
    })
    products.value.unshift(response.data)
    console.log('Product added successfully!')
    editDialog.value = false
  } catch (error) {
    console.error('Error adding product:', error)
  }
}


async function updateProduct(updatedProduct) {
  try {
    const response = await api.put(`/products/${updatedProduct.id}`, {
      ...updatedProduct,
      image: updatedProduct.image || 'https://i.postimg.cc/x1GYVD1p/goldis-logo-horizontal.png',
      rate: updatedProduct.rate || 5,
    })

    await fetchProducts()
    console.log('Product updated successfully!', response.data)
    editDialog.value = false
  } catch (error) {
    console.error('Error updating product:', error)
  }
}


async function deleteProduct(id) {
  try {
    await api.delete(`/products/${id}`)
    products.value = products.value.filter(product => product.id !== id)
    console.log('Product deleted successfully!')
  } catch (error) {
    console.error('Error deleting product:', error)
  }
}
</script>
