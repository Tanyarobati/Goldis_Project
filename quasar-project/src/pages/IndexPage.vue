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
        >
          <img :src="image" alt="carousel image" class="carousel-image" />
        </q-carousel-slide>
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
          <ProductCard
            :product="product"
            :actions="{ addToCart: true, details: true }"
          />
        </q-col>
      </q-row>
    </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { api } from 'boot/axios'
import ProductCard from 'src/components/ProductCard.vue'

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
  height: 400px;
}

.carousel-image {
  width: 100%;
  height: 100%;
  object-fit:fill; /* Ensures the image fits inside the container without cropping */
  border-radius: 15px;

}
</style>
