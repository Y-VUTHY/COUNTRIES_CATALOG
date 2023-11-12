<template>
  <div class="country-list">
    <CountryCard v-for="(country, index) in countries" :key="index" :country="country" />
  </div>
</template>
<script>
import CountryCard from './CountryCard.vue'



export default {
  name: 'CountryList',
  components: {
    CountryCard
  },
  props: {
    countries: {
      type: Array,
      required: true,
    }
  },
  data() {
    return {}
  }
}
</script>
<style scoped>
.country-list {
  height: auto;
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: flex-start;
  z-index: 1;
  box-sizing: border-box;
  background: #000;
}
</style>
