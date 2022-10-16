<template>
  <div class="countries">
    <country
      v-for="country in countries"
      :key="country.name"
      :country="country"
    />
  </div>
</template>

<script>
import Country from "./Country.vue";
export default {
  components: { Country },
  props: {
    countries: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
.countries {
  display: flex;
  flex-wrap: wrap;
  margin: 4rem;
}
</style>