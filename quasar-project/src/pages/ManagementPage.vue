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
      @onSubmit="updateProduct"
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
const selectedProduct = ref(null)
async function fetchProducts() {
  const { data } = await api.get('/products')
  products.value = data
}
fetchProducts()

function openEditDialog(product) {
  selectedProduct.value = product
  editDialog.value = true
}
async function updateProduct(updatedProduct) {
  try {
    // Send updated product data directly (no FormData needed)
    const response = await api.put(`/products/${updatedProduct.id}`, {
      name: updatedProduct.name,
      price: updatedProduct.price,
      sold: updatedProduct.sold,
      description: updatedProduct.description,
      image: updatedProduct.image || 'https://i.postimg.cc/x1GYVD1p/goldis-logo-horizontal.png', // Ensure this is a URL string
      rate: updatedProduct.rate || 5, // Default to 5 if not provided
    });

    // Fetch updated list of products
    await fetchProducts();
    console.log('Product updated successfully!', response.data);
  } catch (error) {
    console.error('Error updating product:', error);
  }
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
