<template>
  <div class="category-container">
    <div v-if="!isMobile" class="category-filters">
      <button
        v-for="(filter, index) in filters"
        :key="index"
        :class="{'category-filters__button': true, 'active-button': filter === activeFilter}"
        @click="applyFilter(filter)"
      >
        {{ capitalizeFirstLetter(filter) }}
      </button>
    </div>

    <div v-else class="category-select">
      <select v-model="activeFilter" @change="applyFilter(activeFilter)">
        <option v-for="(filter, index) in filters" :key="index" :value="filter">
          {{ capitalizeFirstLetter(filter) }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "CategoryFilter",
  data() {
    return {
      filters: [],
      activeFilter: "All",
      isMobile: false,
    };
  },
  created() {
    this.fetchCategories();
    this.checkMobileView();
  },
  mounted() {
    window.addEventListener('resize', this.checkMobileView);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.checkMobileView);
  },
  methods: {
    async fetchCategories() {
      try {
        const response = await axios.get("https://fakestoreapi.com/products/categories");
        this.filters = ["All", ...response.data];
        this.setFilterFromQuery();
      } catch (error) {
        console.error("Error fetching categories:", error);
      }
    },
    applyFilter(filter) {
      this.activeFilter = filter;
      this.$router.push({ query: { filter } });
    },
    setFilterFromQuery() {
      const filterFromUrl = this.$route.query.filter;
      if (filterFromUrl && this.filters.includes(filterFromUrl)) {
        this.activeFilter = filterFromUrl;
      }
    },
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1).toLowerCase();
    },
    checkMobileView() {
      this.isMobile = window.innerWidth <= 480;
    }
  },
  watch: {
    '$route.query.filter': 'setFilterFromQuery',
  }
};

</script>
<style lang="scss" scoped>
.category-container {
  width: 100%;
  height: 64px;
  border-bottom: 1px solid rgba(26, 26, 26, 0.1);

  .category-filters {
    display: flex;
    justify-content: start;
    align-items: center;
    gap: 18px;
    width: 90%;
    margin-inline: auto;
    height: 100%;

    &__button {
      height: 100%;
      background: transparent;
      cursor: pointer;
      outline: none;
      border: none;
      font-weight: 600;
      color: rgba(26, 26, 26, 0.5);
      padding-bottom: 8px;
    }

    .active-button {
      color: rgba(26, 26, 26, 1);
      border-bottom: 3px solid rgba(26, 26, 26, 1);
    }
  }

  .category-select {
    width: 90%;
    margin-inline: auto;

    select {
      width: 100%;
      padding: 8px;
      font-size: 16px;
      border-radius: 4px;
      border: 1px solid rgba(26, 26, 26, 0.2);
    }
  }
}

</style>
