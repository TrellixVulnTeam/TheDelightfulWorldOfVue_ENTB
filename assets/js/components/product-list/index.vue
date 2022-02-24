<template>
  <div class="row">
    <div class="col-12">
      <div class="mt-4">
        <loading v-show="loading" />
        <h5 v-show="!loading && products.length === 0">
          Whoopsie Daisy, no products found!
        </h5>
      </div>
    </div>
    <product-card
      v-for="product in products"
      :key="product['@id']"
      v-show="!loading"
      :product="product"
    />
  </div>
</template>

<script>
import ProductCard from '@/components/product-list/product-card.vue';
import Loading from '@/components/loading.vue';

export default {
  name: 'Product List',
  components: {
    ProductCard,
    Loading,
  },
  props: {
    products: {
      type: Array,
      required: true,
    },
    loading: {
      type: Boolean,
      required: true,
    },
  },
};
</script>
