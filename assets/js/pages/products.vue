<template>
  <div class="container-fluid">
    <div class="row">
      <aside :class="asideClass">
        <sidebar
          :collapsed="sidebarCollapsed"
          :current-category-id="currentCategoryId"
          :categories="categories"
          v-on:toggle-collapsed="toglleSidebarCollapsed"
        />
      </aside>
      <div :class="contentClass">
        <catalog
          :current-category-id="currentCategoryId"
          :categories="categories"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Catalog from '@/components/catalog.vue';
import Sidebar from '@/components/sidebar.vue';
import { getCurrentCategoryId } from '@/services/page-context';
import { fetchCategories } from '@/services/categories-service.js';

export default {
  name: 'Products',
  components: {
    Catalog,
    Sidebar,
  },
  data() {
    return {
      sidebarCollapsed: false,
      categories: [],
      currentCategoryId: getCurrentCategoryId(),
    };
  },
  computed: {
    asideClass() {
      return this.sidebarCollapsed ? 'aside-collapsed' : 'col-xs-12 col-3';
    },
    contentClass() {
      return this.sidebarCollapsed ? 'col-xs-12 col-11' : 'col-xs-12 col-9';
    },
  },
  methods: {
    toglleSidebarCollapsed() {
      this.sidebarCollapsed = !this.sidebarCollapsed;
    },
  },
  async created() {
    // this.categories = fetchCategories();

    const response = await fetchCategories();
    // console.log(response.data);
    this.categories = response.data['hydra:member'];
  },
};
</script>
