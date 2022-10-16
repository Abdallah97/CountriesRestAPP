<template>
  <navbar />
  <div v-if="loaded">
    <search-bar
      v-if="!error"
      @search="searchFilter"
      @selectRegion="filterByRegion"
    />
    <countries v-if="!error" :countries="countries" />
    <div v-else>
      <error />
      <button class="reset-btn" @click="reset">Reset</button>
    </div>
  </div>
  <loading-spinner v-else />
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import SearchBar from "@/components/SearchBar.vue";
import Countries from "@/components/Countries.vue";
import LoadingSpinner from "@/components/LoadingSpinner.vue";
import Error from "@/components/Error.vue";
export default {
  components: { Navbar, SearchBar, Countries, LoadingSpinner, Error },
  data() {
    return {
      countries: [],
      loaded: false,
      error: false,
      prevSearches: [],
    };
  },
  async created() {
    const res = await fetch("https://restcountries.com/v2/all");
    this.countries = await res.json();
    this.loaded = true;
  },

  methods: {
    searchFilter(search) {
      if (search) {
        fetch("https://restcountries.com/v3.1/name/" + search)
          .then((response) => response.json())
          .then((data) => {
            const { status } = data;
            if (status === 404) {
              this.countries = [];
              this.error = true;
            } else {
              this.countries = data;
              this.loaded = true;
              this.error = false;
              //save search to local storage
              //check if search is already in local storage
              if (localStorage.getItem("searches")) {
                this.prevSearches = JSON.parse(
                  localStorage.getItem("searches")
                );
                if (!this.prevSearches.includes(search)) {
                  this.prevSearches.push(search);
                  localStorage.setItem(
                    "searches",
                    JSON.stringify(this.prevSearches)
                  );
                }
              } else {
                this.prevSearches.push(search);
                localStorage.setItem(
                  "searches",
                  JSON.stringify(this.prevSearches)
                );
              }
            }
          })
          .catch((error) => {
            console.log("THE ERROR::", error);
          });
      }
    },
    filterByRegion(region) {
      if (region == "all") {
        fetch("https://restcountries.com/v2/all")
          .then((response) => response.json())
          .then((data) => {
            this.countries = data;
            this.loaded = true;
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        fetch("https://restcountries.com/v2/region/" + region)
          .then((response) => response.json())
          .then((data) => {
            this.countries = data;
            this.loaded = true;
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    reset() {
      fetch("https://restcountries.com/v2/all")
        .then((response) => response.json())
        .then((data) => {
          this.countries = data;
          this.loaded = true;
          this.error = false;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
<style scoped>
.reset-btn {
  /* center the button */
  display: block;
  margin: 0 auto;
  margin-top: 2rem;
  padding: 1rem 2rem;
  border: none;
  border-radius: 5px;
  background-color: rgb(7, 0, 0);
  font-size: 1.6rem;
  font-weight: 400;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
</style>
