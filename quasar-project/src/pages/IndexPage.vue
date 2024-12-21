<template>
  <q-page>
    <div class="q-pa-md q-pb-xl">


      <q-carousel
        animated
        v-model="slide"
        arrows
        navigation
        infinite
        class="custom-carousel "
      >
        <q-carousel-slide
          v-for="(image, index) in carouselImages"
          :key="index"
          :name="index"
          :img-src="image"
        />
      </q-carousel>


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
            <q-card class="my-card">
              <q-img
                :src="product.image || 'https://via.placeholder.com/250x250.png?text=No+Image'"
                alt="Product Image"
                class="q-mb-md"
                :lazy="true"
                style="max-width: 100%; max-height: 200px; object-fit: cover;"
              />


              <q-card-section>
                <div class="text-h6">{{ product.name }}</div>
                <div class="text-subtitle2 text-grey">{{ product.sold }}</div>
              </q-card-section>

              <q-card-section align="right">
                <div>
                  <span class="text-bold">قیمت:</span> ${{ product.price }}
                </div>
                <q-rating
                  :value="product.rating"
                  size="18px"
                  color="yellow"
                />
              </q-card-section>

              <q-card-actions align="around">
                <q-btn label="جزئیات" flat />
                <q-btn label="افزودن به سبد خرید" color="teal-10" icon="shopping_cart" />
              </q-card-actions>
            </q-card>
          </q-col>
        </q-row>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { api } from 'boot/axios'

const slide = ref(1)
const carouselImages = ref([])
const products = ref([])

async function fetchProducts() {
  const { data } = await api.get('/products')
  products.value = data
  carouselImages.value = data.map((p) => p.image)
}
fetchProducts()
</script>

<style>
.custom-carousel {
  width: 100%;
  height: 800px;
  border-radius: 10px;
}

.my-card {
  max-width: 250px;
  min-width: 200px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

.product-col {
  display: flex;
  justify-content: center;
}
</style>
