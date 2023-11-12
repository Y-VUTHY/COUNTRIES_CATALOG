<template>
  <div class="countries-catalog">
    <header>
      <h1>Countries Catalog</h1>

      <div class="search-tools">
        <el-row>
          <el-input class="search-input" v-model="searchCountry" placeholder="Search">
            <template #prepend>
              <el-button class="search-icon" :icon="searchIcon" />
            </template>
          </el-input>
        </el-row>
      </div>

      <div class="sort-tools">
        <span>Sort by name</span>
        <el-row>
          <el-button size="small" round @click="ascendingCountries">A To Z</el-button>
          <el-button size="small" round @click="descendingCountries">Z To A</el-button>
        </el-row>
      </div>
    </header>

    <CountryList v-if="countries && 0 < countries.length" :countries="countries" />
    <template v-if="isLoading">
      <el-skeleton :rows="5" animated style="{ width: '100%' }" />
    </template>

    <template v-if="countries && 0 === countries.length && !isLoading">
      <el-empty :image-size="100" />
    </template>

    <el-pagination
      v-if="countries && 0 < countries.length"
      :pager-count="3"
      layout="prev, pager, next"
      :total="isSearch ? countries.length : countriesLength"
      :page-size="pageSize"
      small
      background
      @current-change="handleCurrentChange"
    />
    <el-pagination />
  </div>
</template>

<script>
import axios from 'axios'
import { debounce, isEmpty } from 'lodash'
import { Search } from '@element-plus/icons-vue'

import CountryList from './CountryList.vue'

const COUNTRY_API_URL = 'https://restcountries.com/v3.1'

export default {
  name: 'CountriesCatalog',
  components: {
    CountryList
  },
  data() {
    return {
      countries: [],
      pageSize: 25,
      currentPage: 1,
      isSearch: false,
      isLoading: false,
      searchCountry: '',
      countriesLength: 0,
      searchIcon: Search,
      originalCountries: []
    }
  },
  methods: {
    async performSearch() {
      if (true === isEmpty(this.searchCountry)) {
        this.isSearch = false
        this.updateDisplayedCountries()
        return
      }
      try {
        this.isSearch = true
        const response = await axios.get(`${COUNTRY_API_URL}/name/${this.searchCountry}`)
        this.countries = response.data?.slice(0, 25)
      } catch (error) {
        this.countries = []
      }
    },
    ascendingCountries() {
      this.countries.sort((a, b) => a.name.official.localeCompare(b.name.official))
    },
    descendingCountries() {
      this.countries.sort((a, b) => b.name.official.localeCompare(a.name.official))
    },
    handleCurrentChange(newPage) {
      this.currentPage = newPage
      this.updateDisplayedCountries()
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      })
    },
    updateDisplayedCountries() {
      const startIndex = (this.currentPage - 1) * this.pageSize
      const endIndex = startIndex + this.pageSize
      this.countries = this.originalCountries.slice(startIndex, endIndex)
      this.isLoading = false
    }
  },
  watch: {
    searchCountry: debounce(function () {
      this.performSearch()
    }, 2000)
  },
  async mounted() {
    try {
      this.isLoading = true
      const response = await axios.get(`${COUNTRY_API_URL}/all`)
      this.countriesLength = response.data?.length
      this.originalCountries = response.data
      this.updateDisplayedCountries()
    } catch (error) {
      console.error(error)
    }
  }
}
</script>

<style scoped lang="scss">
.countries-catalog {
  background: inherit;
}
.countries-catalog header {
  position: sticky;
  top: 0rem;
  z-index: 2;
  padding: 1rem 0 0.7rem 0;
  background: black;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}
header > h1 {
  color: #ff1584;
  font-weight: bold;
}
header .search-tools {
  height: 1.4rem;
}
.search-tools > .el-row,
.search-input {
  height: 100%;
}
.search-input .el-input__inner {
  height: 100%;
}
header .sort-tools {
  display: flex;
  flex-direction: column;
  height: auto;
  gap: 00.2rem;
  margin-top: 0.5rem;
}
.sort-tools > span {
  color: rgb(167, 167, 167);
  font-family: Arial, Helvetica, sans-serif;
  padding: 0;
  margin: 0;
}
.sort-tools > .el-row {
  display: flex;
  gap: 0.3rem;
}
.sort-tools > .el-row > .el-button {
  margin: 0;
  font-size: 10px;
  height: 1.2rem;
  padding: 0 0.3rem;
}
.el-pagination {
  margin: 0.5rem 0;
}
</style>
