<template>
  <div>
    <div class="row">
      <div class="col-12">
        <h1>Products</h1>
      </div>
    </div>
    <product-list :products="products" :loading="loading" />
    <div class="row">
      <legend-component :title="legend" />
    </div>
  </div>
</template>

<script>
import LegendComponent from '@/components/legend.vue';
import ProductList from '@/components/product-list';
import { fetchProducts } from '@/services/product-services.js';

export default {
  name: 'Catalog',
  components: {
    LegendComponent,
    ProductList,
  },
  data() {
    return {
      legend: "Shipping takes 10-12 weeks, and products probably won't work",
      products: [],
      loading: false,
    };
  },
  props: {
    currentCategoryId: {
      type: String,
      default: null,
    },
  },
  async created() {
    // axios.get('/api/products').then((response) => {
    //   console.log(response);
    // });
    this.loading = true;

    let response;
    try {
      response = await fetchProducts(this.currentCategoryId);
    } catch (e) {
      this.loading = false;
      return;
    }

    this.loading = false;
    this.products = response.data['hydra:member'];
  },
};
</script>
