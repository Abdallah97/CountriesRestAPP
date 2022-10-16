<template>
  <Navbar />
  <div class="container" v-if="loaded">
    <div class="country-details">
      <div class="country-details__back">
        <router-link to="/">
          <i class="fa fa-long-arrow-left" aria-hidden="true"></i>
          Back
        </router-link>
      </div>
    </div>
    <div class="country-details__view">
      <img
        width="50rem"
        class="country-details__view img"
        :src="country.flags['png'] ? country.flags['png'] : country.flag"
        alt="name"
      />

      <div class="country-info">
        <div class="country-info__left">
          <h2 class="country-info__title">{{ country.name }}</h2>
          <p class="country-info__details">
            <span class="country-info__details__title">Native name:</span>
            {{ country.nativeName }}
          </p>
          <p class="country-info__details">
            <span class="country-info__details__title">Population:</span>
            {{ country.population }}
          </p>
          <p class="country-info__details">
            <span class="country-info__details__title"
              >Region: {{ country.region }}</span
            >
            Europe
          </p>
          <p class="country-info__details">
            <span class="country-info__details__title">Subregion: </span>
            {{ country.subregion }}
          </p>
          <p class="country-info__details">
            <span class="country-info__details__title">Capital: </span>
            {{ country.capital }}
          </p>
        </div>

        <div class="country-info__right">
          <p class="country-info__details">
            <span class="country-info__details__title">Top Level Domain:</span>
            <span
              style="font-size: 1.6rem"
              class=""
              v-for="(domain, index) in country.topLevelDomain"
              :key="index"
            >
              {{ domain }}
            </span>
          </p>

          <p class="country-info__details">
            <span class="country-info__details__title">Languages: </span>
            <span
              style="font-size: 1.6rem"
              class=""
              v-for="(lang, index) in country.languages"
              :key="index"
            >
              {{ lang.name }}
            </span>
          </p>
        </div>
      </div>
      <p class="country-info__details borders">
        <span class="country-info__details__title">Borders: </span>
        <button
          v-for="(border, index) in country.borders"
          :key="index"
          class="border-btn"
          @click="getBorderDetails(border)"
        >
          {{ border }}
        </button>
      </p>
    </div>
  </div>

  <LoadingSpinner v-else />
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import LoadingSpinner from "../components/LoadingSpinner.vue";
export default {
  components: { Navbar, LoadingSpinner },
  data() {
    return {
      country: [],
      loaded: false,
    };
  },
  props: ["name"],
  mounted() {
    fetch(`https://restcountries.com/v2/name/${this.name}`)
      .then((res) => res.json())
      .then((data) => {
        this.country = data[0];
        this.loaded = true;
        console.log(this.country);
      });
  },

  methods: {
    getBorderDetails(border) {
      fetch(`https://restcountries.com/v2/alpha/${border}`)
        .then((res) => res.json())
        .then((data) => {
          debugger;
          this.country = data;
          this.loaded = true;
          this.$router.push(`/country/${border}`);
          console.log(this.country);
        });
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
}
.country-details {
  margin: 4rem;
  display: flex;
  align-items: flex-start;
}
.country-details__back a {
  margin-bottom: 4rem;
  /* style as a button  */
  padding: 1rem 2rem;
  border-radius: 5px;
  background-color: hsl(0, 0%, 98%);
  text-decoration: none;
  font-size: 1.4rem;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  color: #000;
}
.fa-long-arrow-left {
  margin-right: 1rem;
}
.country-details__view {
  margin: 2rem;
}
.country-details__view img {
  width: auto;
  margin-bottom: 1rem;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.country-info {
  display: flex;
  float: right;
  padding: 4rem;
  width: 60%;
}
.country-info__title {
  font-size: 2.4rem;
  font-weight: 900;
  text-align: left;
  margin-bottom: 2rem;
}
.country-info__details {
  font-size: 1.6rem;
  font-weight: 400;
  margin: 1rem 0;
  position: relative;
}
.country-info__details__title {
  font-weight: 900;
  font-size: 1.6rem;
}
.country-info__left {
  margin-right: 20rem;
}
.country-info__right {
  align-self: center;
}
.content {
  margin: 4rem;
  transform: translate(0, 50%);
}
.country-info__details__border {
  flex: 1 1 100%;
  display: flex;
  justify-content: space-between;
  margin: 1rem 0;
}
.btn {
  padding: 0.5rem 3rem;
  border-radius: 5px;
  background-color: hsl(0, 0%, 98%);
  text-decoration: none;
  font-size: 1.4rem;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  color: #000;
  margin: 1rem;
  border: none;
  outline: none;
}
.borders {
  float: right;
  width: 50%;
}
.border-btn {
  padding: 0.5rem 3rem;
  border-radius: 5px;
  background-color: hsl(0, 0%, 98%);
  font-size: 1.4rem;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  color: #000;
  margin: 1rem;
  border: none;
  outline: none;
  cursor: pointer;
}
</style>