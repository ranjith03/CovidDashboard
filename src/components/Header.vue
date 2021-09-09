<template>
  <div class="container container-wide">
    <header class="header">
      <div class="header-title">
        <img v-if="visible" src="../assets/logo.png" class="country-flag" width="52" />
        <img v-if="flagvisible" :src='flagdata' class="country-flag" width="52" />
        
        <div class="right">
          <h1 v-if="selectedCountry">
            {{ selectedCountry.country }} <span>Overview</span>
          </h1>
          <h1 v-else>Covid-19<span> Global Statistics</span></h1>
          <span class="date">{{ getDataDate }}</span>
        </div>
      </div>
      <Search
        :items="getCountries"
        filterBy="country"
        @selectedItem="onSelected"
      />
    </header>
  </div>
</template>

<script>
import Search from "@/components/Search";
import { mapGetters } from "vuex";

export default {
  name: "Header",
  components: {
    Search
  },
  data() {
    return {
      baseUrl:"https://restcountries.eu/data/",
      selectedCountry: null,
      flagdata:"",
      visible:true,
      flagvisible:false,
    };
  },
  computed: {
    ...mapGetters(["getCountries", "getDataDate"]),
    
  },
  methods: {
    onSelected(value) {
      this.selectedCountry = value;
      this.$emit("selectedItem", this.selectedCountry);
      this.flagdata = this.baseUrl + this.selectedCountry.alpha3Code.toLowerCase() + '.svg';
      this.visible=false;
      this.flagvisible = true;
    },
  }
};
</script>

<style scoped lang='scss'>
.header {
  background-color: var(--secondary-color);
  height: var(--header-height);
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom-left-radius: var(--border-radius);
  border-bottom-right-radius: var(--border-radius);
  box-shadow: 0 25px 35px rgba(0, 0, 0, 0.07);
  padding-left: 30px;
  padding-right: 30px;

  .header-title {
    display: flex;
    align-items: center;
    .right {
      margin-left: 10px;
      line-height: 20px;
    }
    h1 {
      font-size: 20px;
      font-weight: 800;
      color: rgb(var(--gray-1));
      span {
        font-weight: 500;
      }
    }
    .date {
      font-size: 13px;
      font-weight: 500;
      color: rgb(var(--gray-1));
    }

    @media only screen and (max-width: 600px) {
      margin-bottom: 10px;
      h1 {
        span {
          display: block;
        }
      }
    }
  }

  @media only screen and (max-width: 600px) {
    flex-direction: column;
    height: auto;
    padding: 20px 30px;
    .header-title {
      h1 {
        font-size: 18px;
      }
    }
  }
}
</style>
