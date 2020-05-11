
<template>
<div>
      <line-chart v-if="loaded" :chartdata="chartdata" :options="options" />
  <select v-model="country">
  <option disabled value="">Please select one</option>
  <option>South Africa</option>
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
    country: null
  }),
  methods: {
    getCountryLabel: function(countryData, totalConfirmedAllCountries) {
      const percentageOfCases = (
        (countryData.Confirmed / totalConfirmedAllCountries) *
        100
      ).toFixed(2);
      return `${countryData.Country}: ${percentageOfCases}% (${countryData.Confirmed})`;
    },
  },
  async mounted() {
    this.loaded = false;
    try {
      fetch(`https://api.covid19api.com/total/dayone/country/south-africa`)
        .then(response => response.json())
        .then(summaryData => {
          const filteredSummaryData = summaryData.Countries.map(sd => {
              return {
                Date: sd.Date,
                Confirmed: sd.Confirmed
              };
            }) 
          const totalConfirmed = filteredSummaryData.reduce(
            (a, b) => a + b.Confirmed,
            0
          );
          this.chartdata = {
            labels: filteredSummaryData.map(sd =>
              this.getCountryLabel(sd, totalConfirmed)
            ),
            datasets: [
              {
                backgroundColor: filteredSummaryData.map(this.getRandomHex),
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
};
</script>

<style>
  /* #pie-chart {
    width: 50% !important;
    height: 50% !important;
  } */
</style>

