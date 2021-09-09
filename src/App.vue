<template>
  <div class="app">
    <main class="main">
      <Header @selectedItem="onSelected" />
      <Summary :selectedItem="selectedItem" />
      <Chart :selectedItem="selectedItem" />
      <Table />
      <Footer />
    </main>
  </div>
</template>

<script>
import Header from "@/components/Header";
import Summary from "@/components/Summary";
import Chart from "@/components/Chart";
import Table from "@/components/Table";
import Footer from "@/components/Footer";

import { mapActions } from "vuex";

export default {
  name: "App",
  components: {
    Header,
    Summary,
    Chart,
    Table,
    Footer
  },
  data() {
    return {
      selectedItem: null
    };
  },
  methods: {
    ...mapActions(["fetchCountries", "fetchSummaryData"]),
    onSelected(value) {
      this.selectedItem = value;
    }
  },
  created() {
    this.fetchCountries();
    this.fetchSummaryData();
  }
};
</script>

<style scoped lang='scss'>

  @media only screen and (max-width: 992px)  {
    .main {
    padding-left: 10px;
    padding-right: 10px;
  }
}
</style>
