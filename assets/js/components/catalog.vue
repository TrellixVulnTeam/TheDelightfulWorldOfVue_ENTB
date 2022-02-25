<template>
  <div>
    <div class="row">
      <div class="col-3">
        <title-component
          :current-category-id="currentCategoryId"
          :categories="categories"
        />
      </div>
      <div class="col-9">
        <search-bar @search-products="onSearchProducts" />
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
import TitleComponent from '@/components/title.vue';
import SearchBar from './search-bar.vue';

export default {
  name: 'Catalog',
  components: {
    LegendComponent,
    ProductList,
    TitleComponent,
    SearchBar,
  },
  data() {
    return {
      legend: "Shipping takes 10-12 weeks, and products probably won't work",
      products: [],
      loading: false,
      searchTerm: null,
    };
  },
  props: {
    currentCategoryId: {
      type: String,
      default: null,
    },
    categories: {
      type: Array,
      required: true,
    },
  },
  watch: {
    currentCategoryId() {
      this.loadProducts();
    },
  },
  created() {
    // axios.get('/api/products').then((response) => {
    //   console.log(response);
    // });
    this.loadProducts();
  },
  methods: {
    /**
     * Handles a change in the searchTerm provided by the search bar and fecheches a new set
     *
     * @param {string} term
     */
    onSearchProducts({ term }) {
      this.searchTerm = term;
      this.loadProducts();
    },
    async loadProducts() {
      this.loading = true;

      let response;
      try {
        response = await fetchProducts(this.currentCategoryId, this.searchTerm);
      } catch (e) {
        this.loading = false;
        return;
      }

      this.loading = false;
      this.products = response.data['hydra:member'];
    },
  },
};
</script>
