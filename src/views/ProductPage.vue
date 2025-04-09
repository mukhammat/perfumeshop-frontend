<template>
  <div>
    <ProductDetails :product="product" />
  </div>
</template>

<script>
import axios from 'axios';
import ProductDetails from './components/ProductDetails.vue';

export default {
  components: {
    ProductDetails
  },
  data() {
    return {
      product: null
    };
  },
  async created() {
    const productId = this.$route.params.id;
    try {
      const response = await axios.get(`http://172.20.10.11:3000/api/perfume/get-one/${productId}`);
      this.product = response.data;
    } catch (error) {
      console.error('Ошибка при получении данных о товаре:', error);
    }
  }
}
</script>

