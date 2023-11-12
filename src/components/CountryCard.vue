<template>
  <el-card class="country-card" :body-style="{ padding: '0px' }">
    <img :src="country.flags.png" class="image" alt="flag-png" />
    <div class="information">
      <CountryModal :country="country" />
      <div class="cca-code">
        <span> <span class="title">CCA2:</span> {{ country.cca2 }}</span>
        <span><span class="title">CCA3:</span> {{ country.cca3 }}</span>
      </div>
      <div class="native-name">
        <span class="title">Native name:</span>
        <div class="native-name-list">
          <span v-for="(value, key) in country.name.nativeName" :key="key">
            <span class="title">{{ key }}:</span>
            <div class="native-name-data" v-for="(data, type) in value" :key="type">
              <p>
                <el-icon><CaretRight /></el-icon> {{ type }}: {{ data }}
              </p>
            </div>
          </span>
        </div>
      </div>

      <span>
        <span class="title">altSpellings: </span>
        <span v-for="(altSpelling, index) in country.altSpellings.slice(0, 2)" :key="index">
          {{ altSpelling }}{{ 1 <country.altSpellings?.length ? ', ': '' }}
        </span>
        <span> {{ 1 < country.altSpellings?.length ? ' etc.' : '' }}</span>
      </span>
      <span
        ><span class="title">idd:</span>
        <div class="idd-item">
          <p>
            <el-icon><CaretRight /></el-icon> root: {{ country.idd.root? country.idd.root: 'N/A' }}
          </p>
          <p>
            
            <el-icon><CaretRight /></el-icon><span>suffixes:</span>
            <span v-for="(suffixe, index) in country.idd.suffixes" :key="index">
               {{suffixe}} {{ 1 < country.idd.suffixes?.length ? ',': '' }}
            </span>
            <span v-if="!country.idd.suffixes">N/A</span>
            <spane v-else> {{ 1 < country.idd.suffixes?.length ? ' etc.' : '' }}</spane>
          </p>
        </div>
      </span>
    </div>
  </el-card>
</template>

<script>
import { CaretRight } from '@element-plus/icons-vue'

import CountryModal from './CountryModal.vue'

export default {
  name: "CountryCard",
  components: {
    CountryModal,
    CaretRight
  },
  props: {
    country: {
      type: Object,
      required: true,
    }
  },
  data() {
    return {}
  },
}
</script>

<style scoped>
.country-card {
  width: 23%;
  height: auto;
  color: rgb(98, 98, 98);
  @media screen  and (min-width: 1440px){
    width: 24%;
    .information span{
      font-size: 16px;
    }
  }
   @media screen  and (max-width: 1440px){
    width: 24%;
    .information span{
      font-size: 14px;
    }
  }
  @media screen  and (max-width: 1262px){
    width: 23%;
    .information span{
      font-size: 13px;
    }
  }
  @media screen  and (max-width: 1000px){
    width: 32%;
  }
  @media screen  and (max-width: 864px){
    width: 48%;
  }
 
   @media screen  and (max-width: 530px){
   width: 100%;
  }
}

img {
  width: 100%;
  height: 8rem;
  display: block;
  border: 0.01rem solid;
  @media screen and (min-width: 1440px){
    height: 16rem;
  }
   @media screen  and (max-width: 1440px){
    height: 12rem;
  }
  @media screen  and (max-width: 1262px){
    height: 11rem;
  }
   @media screen  and (max-width: 1135px){
    height: 9rem;
  }
   @media screen  and (max-width: 1030px){
    height: 7rem;
  }
   @media screen  and (max-width: 1000px){
    height: 9rem;
  }
   @media screen  and (max-width: 864px){
    height: 11rem;
  }
  
  @media screen  and (max-width: 530px){
   height: 14rem;
  }
}
.information {
  padding: 0.3rem;
  height: auto;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}
.information span {
font-size: 10px;
}
.information > .cca-code {
  display: flex;
  gap: 1rem;
}
.information .title {
  font-weight: bold;
}
.information .native-name {
  display: flex;
  flex-direction: column;
}
.information .native-name-list {
  display: flex;
  flex-direction: column;
  margin-left: 0.2rem;
}
.information .native-name-data {
  margin-left: 0.2rem;
}
.native-name-data p{
  display: flex;
  align-items: center;
}
.information .idd-item {
  display: flex;
  flex-direction: column;
  margin-left: 0.2rem;
}
.idd-item p {
  display: flex;
  align-items: center;
}
</style>
