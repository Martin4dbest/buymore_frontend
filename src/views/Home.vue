<template>
  <div class="home">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Latest products</h2>
      </div>

      <ProductBox
        v-for="product in latestProducts"
        :key="product.id"
        :product="product" />

    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox'

export default {
  name: 'Home',
  data() {
    return {
      latestProducts: []
    }
  },
  components: {
    ProductBox
  },
  mounted() {
    this.getLatestProducts()
    document.title = 'Home | Dorbass Optimum Global'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)
      
      try {
        const response = await axios.get('/api/v1/latest-products/')
        this.latestProducts = response.data
      } catch (error) {
        console.error(error)
      }

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

.home {
   /* Professional blue color */
  height: 550px; /* Set a fixed height */
  display: flex; /* Flexbox for centering content */
  align-items: center; /* Center vertically */
  justify-content: center; /* Center horizontally */
  border-radius: 40px; /* Rounded corners */
  overflow: hidden; /* Ensure content doesn't overflow */
}

.home::before {
  content: ""; /* Create a pseudo-element for styling */
  display: block;
  padding-top: 55%; /* Maintain aspect ratio (4:3) */
}
</style>
