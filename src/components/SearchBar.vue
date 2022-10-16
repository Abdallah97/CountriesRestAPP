<template>
  <div class="container">
    <div class="search-bar">
      <i @click="search" class="fa fa-search" aria-hidden="true"> </i>
      <input
        type="text"
        placeholder="Search for a country..."
        v-model="searchText"
        @keyup.enter="search"
        @click="searchResultsToggle = !searchResultsToggle"
      />
    </div>
    <div v-if="searchResultsToggle" class="previous-searches-list">
      <ul class="searches">
        <li
          v-for="search in prevSearches"
          :key="search"
          class="search"
          @click="searchText = search"
        >
          {{ search }}
        </li>
      </ul>
    </div>

    <div class="filter">
      <select
        class="filter-menu"
        v-model="region"
        @change="selectRegion"
        name="filter"
      >
        <option value="all">Filter by Region</option>
        <option value="Africa">Africa</option>
        <option value="Americas">Americas</option>
        <option value="Asia">Asia</option>
        <option value="Europe">Europe</option>
        <option value="Oceania">Oceania</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchText: "",
      region: "all",
      prevSearches: [],
      searchResultsToggle: false,
    };
  },
  mounted() {
    if (localStorage.getItem("searches")) {
      this.prevSearches = JSON.parse(localStorage.getItem("searches"));
    }
  },
  updated() {
    this.prevSearches = JSON.parse(localStorage.getItem("searches")) || [];
  },

  methods: {
    search() {
      this.$emit("search", this.searchText);
      this.searchText = "";
    },
    selectRegion() {
      this.$emit("selectRegion", this.region);
    },
  },
};
</script>

<style scoped  >
.container {
  display: flex;
  justify-content: space-between;
}
.search-bar {
  margin: 4rem;
}
input {
  min-width: 40rem;
  padding: 1rem 1.5rem 1rem 2.5rem;
  border: 1px solid hsl(0, 0%, 80%);
  border-radius: 5px;
  font-size: 1.6rem;
  position: relative;
  outline: none;
}
.filter {
  margin: 4rem;
}
.filter-menu {
  min-width: 20rem;
  padding: 1rem 1.5rem 1rem 1.5rem;
  border: 1px solid hsl(0, 0%, 80%);
  border-radius: 5px;
  font-size: 1.6rem;
  outline: none;
}
.filter-menu option {
  font-size: 1.6rem;
  outline: none;
}
.fa-search {
  position: absolute;
  top: 16.6rem;
  left: 4.5rem;
  z-index: 1;
  font-size: 1.6rem;
  color: hsl(0, 0%, 60%);
  cursor: pointer;
}
.previous-searches-list {
  /* show as a list below input search */
  position: absolute;
  box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.1);
  top: 20rem;
  left: 4.5rem;
  z-index: 1;
  background: white;
  font-size: 1.6rem;
  color: hsl(0, 0%, 60%);
  list-style: none;
  width: 39rem;
  cursor: pointer;
}
.previous-searches-list li {
  padding: 1rem 1.5rem 1rem 1.5rem;
  width: 100%;
  border-radius: 5px;
  list-style: none;
  font-size: 1.6rem;
  outline: none;
  cursor: pointer;
}
</style>