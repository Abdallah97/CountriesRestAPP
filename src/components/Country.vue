<template>
  <div class="country" @click="showDetails">
    <img
      :src="country.flags['png'] ? country.flags['png'] : country.flag"
      alt="name"
    />
    <div class="country-info">
      <h2>{{ country.name.common ? country.name.common : country.name }}</h2>
      <p><span>Population: </span> {{ country.population }}</p>
      <p><span>Region: </span>{{ country.region }}</p>
      <p>
        <span>Capital: </span
        >{{
          typeof country.capital === "object"
            ? country.capital[0]
            : country.capital
        }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    country: {
      type: Object,
      required: true,
    },
  },
  methods: {
    showDetails() {
      const isFromFilter = this.country.name.common
        ? this.country.name.common
        : this.country.name;
      this.$router.push(`/country/${isFromFilter}`);
    },
  },
};
</script>

<style scoped>
.country {
  width: 25rem;
  margin: 2rem auto;
  margin-right: 2rem;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  margin: 1rem auto;
  cursor: pointer;
}
.country img {
  max-width: 100%;
  height: 18rem;
  margin-bottom: 1rem;
  border-radius: 5px;
}
.country-info {
  margin-left: 4rem;
}
.country-info h2 {
  font-size: 2.4rem;
  font-weight: 900;
}
.country-info p {
  font-size: 1.6rem;
  font-weight: 400;
  margin: 1rem 0;
}
.country-info p span {
  font-weight: 900;
  font-size: 1.6rem;
}
</style>