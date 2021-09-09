<template>
  <div class="container section-gap" v-show="!loader">
    <div class="chart-card">
      <div class="section-title">
        PAST 15 DAYS CHART OF
        <span>{{ selectedItem ? selectedItem.country : "" }}</span>
      </div>
      <VueApexCharts
        height="500"
        type="line"
        :options="options"
        :series="series"
      ></VueApexCharts>
    </div>
  </div>
</template>

<script>
import VueApexCharts from "vue3-apexcharts";
import { mapGetters, mapActions } from "vuex";

export default {
  name: "Chart",
  props: ["selectedItem"],
  components: {
    VueApexCharts
  },

  data() {
    return {
      countryCode: "",
      loader: true,
      series: [],
      options: {
        chart: {
          height: 350,
           type: "line",
          stacked: false
        },
        plotOptions: {
          bar: {
            horizontal: false
          }
        },
        dataLabels: {
          enabled: false
        },
        tooltip: {
          shared: false,
          followCursor: true
        },
        stroke: {
          curve: 'smooth',
        },
        fill: {
          opacity: 1
        },
        legend: {
          position: "top",
          horizontalAlign: "center",
          offsetX: 40
        },
        colors: ["rgb(95, 193, 215)", "rgb(226, 37, 43)", "rgb(94, 181, 89)"]
      }
    };
  },
  computed: {
    ...mapGetters(["getMonthlyData"])
  },
  methods: {
    ...mapActions(["fetchCountryData"]),
    async fetchData() {
      await this.$store.dispatch("fetchCountryData", this.countryCode);
      await this.fillData();
    },
    fillData() {
      const confirmed = this.getMonthlyData.map(item => item.Confirmed);
      const deaths = this.getMonthlyData.map(item => item.Deaths);
      const recovered = this.getMonthlyData.map(item => item.Recovered);
      const date = this.getMonthlyData.map(item => item.Date);
      this.series = [
        {
          name: "Confirmed",
          data: confirmed
        },
        {
          name: "Deaths",
          data: deaths
        },
        {
          name: "Recovered",
          data: recovered
        }
      ];
      this.options = {
        xaxis: {
          categories: date
        }
      };
    }
  },
  watch: {
    selectedItem(val) {
      this.countryCode = val.alpha2Code;
      this.fetchData();
    },
    getMonthlyData() {
      if (this.getMonthlyData.length) {
        this.loader = false;
      } else {
        this.loader = true;
      }
    }
  }
};
</script>

<style scoped lang='scss'>
.chart-card {
  min-height: 400px;
  background-color: var(--secondary-color);
  border-radius: var(--border-radius);
  .section-title {
    text-transform: uppercase;
  }
}
</style>
