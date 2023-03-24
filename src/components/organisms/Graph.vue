<template>
  <div v-if="!!series" id="graph">
    <apexchart
      :type="chartType"
      :options="options"
      :series="series"
      @click="handle"
    ></apexchart>
  </div>
</template>

<script>
import VueApexCharts from "vue-apexcharts";
export default {
  name: "Graph",
  components: {
    apexchart: VueApexCharts,
  },
  data() {
    return {
      categories: [],
      options: {
        chart: {
          type: "",
          events: {},
        },
        xaxis: {
          categories: [],
        },
      },
      series: [],
      tooltip: {
        intersect: true,
        shared: false,
      },
      markers: {
        size: 1,
      },
    };
  },
  props: {
    results: null,
    chartType: {
      type: String,
      default: "line",
    },
    seriesNames: {
      type: Array,
      default: () => [],
    },
    width: {
      type: String,
      default: "auto",
    },
    height: {
      type: String,
      default: "212px",
    },
  },
  computed: {
    getData() {
      return this.results;
    },
  },
  mounted() {
    this.prepareData();
  },
  methods: {
    prepareData() {
      const series = this.getData["Time Series (Daily)"];
      this.categories = Object.keys(series);
      this.options.chart.type = this.chartType;
      this.options.chart.width = this.width;
      this.options.chart.height = this.height;
      this.options.xaxis.categories = this.categories;
      for (const serieName of this.seriesNames) {
        let series_data = Object.values(series).map((serie) => {
          serie = this.json(serie);
          return serie[serieName];
        });
        this.series.push({
          name: serieName,
          data: series_data,
        });
      }
    },
    json(data) {
      return JSON.parse(JSON.stringify(data));
    },
    handle(event, chartContext, config) {
      this.$emit("graph-click", {
        point: config.dataPointIndex,
        name: this.seriesNames[config.seriesIndex],
        index: config.seriesIndex,
      });
    },
  },
};
</script>
