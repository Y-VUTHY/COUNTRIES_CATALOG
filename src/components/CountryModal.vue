<template>
  <div class="country-modal-container">
    <span class="country-name" text @click="centerDialogVisible = true"
      >Name: {{ country.name.official }}</span
    >
    <el-dialog
      v-model="centerDialogVisible"
      :title="country.name.official"
      width="30%"
      destroy-on-close
      center
      class="country-modal"
    >
      <img :src="country.flags.png" alt="flag-png" />
      <p><span class="title">Area: </span>{{ country.area }}</p>
      <p>
        <span class="title">Map: </span>
        <a :href="country.maps.googleMaps" target="_blank">{{ country.maps.googleMaps }}</a>
      </p>
      <p><span class="title">Population: </span>{{ country.population }}</p>
      <p>
        <span class="title">Postal Code: </span
        >{{ country.postalCode?.format ? country.postalCode.format : 'N/A' }}
      </p>
      <p><span class="title">Region: </span>{{ country.region }}</p>
    </el-dialog>
  </div>
</template>
<script>
export default {
  name: 'CountryModal',
  props: {
    country: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      centerDialogVisible: false
    }
  },
  mounted() {}
}
</script>
<style scoped>
.country-modal-container {
  display: flex;
  box-sizing: inherit;
}
.country-modal-container .country-name {
  font-size: 11px;
  width: 100%;
  font-weight: bold;
  cursor: pointer;
  @media screen and (min-width: 1440px) {
    font-size: 16px;
  }
  @media screen and (max-width: 1440px) {
    font-size: 14px;
  }
  @media screen and (max-width: 1262px) {
    font-size: 13px;
  }
}
.el-dialog {
  width: 20rem;
}
</style>
