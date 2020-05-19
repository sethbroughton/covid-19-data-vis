
<template>
<div>
  {{totalConfirmed}}
      <line-chart v-if="loaded" :chartdata="chartdata" :options="options" />

  <select v-model="country">
  <option disabled value="">Please select one</option>
  <option>south-africa</option>
  <option>B</option>
  <option>C</option>
    </select>
</div>
</template>

<script>
import LineChart from "./LineChart.vue";
export default {
  name: "CaseByCountry",
  components: { LineChart },
  data: () => ({
    loaded: false,
    chartdata: null,
    options: null,
    country: '',
    totalConfirmed: 0
  }),
  methods: {
    getRandomHex(){
      return "#" + Math.floor(Math.random() * 16777215).toString(16);
    },
    createChart(){
        try {
      fetch(`https://api.covid19api.com/dayone/country/${this.country}/status/confirmed`)
        .then(response => response.json())
        .then(summaryData => {
          const filteredSummaryData = summaryData.map(sd => {
              return {
                Date: sd.Date,
                Confirmed: sd.Cases
              };
            }) 
          this.totalConfirmed = filteredSummaryData.reduce(
            (a, b) => a + b.Confirmed,
            0
          );

          this.chartdata = {
            labels: filteredSummaryData.map(sd =>
              sd.Date
            ),
            datasets: [
              {
                backgroundColor: '#0388fc',
                data: filteredSummaryData.map(sd => sd.Confirmed)
              }
            ]
          };
          this.options = {
            responsive: false,
            maintainAspectRatio: false
          };
          this.loaded = true;
        });
    } catch (e) {
      console.error(e);
    }
    }

  },
  watch:{
    country: function(){
      this.createChart();
    }

  }
  ,
  async mounted() {
    this.loaded = false;
    this.createChart();
  }
};
</script>

<style>
  /* #pie-chart {
    width: 50% !important;
    height: 50% !important;
  } */
 
</style>

