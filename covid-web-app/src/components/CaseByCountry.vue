
<template>
<div>
      <line-chart v-if="loaded" :chartdata="chartdata" :options="options" />

  <input type="radio"  name="linear" value="linear" v-model="scale"> linear 
    <input type="radio"  value="logarithmic" v-model="scale"> logarithmic 

  <select v-model="country">
    <option disabled value="">Please select one</option>
    <option v-for="country in allCountries" v-bind:key="country.Country" v-bind:value="country.Slug">
    {{country.Country}}
    </option>
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
    allCountries:[],
    scale: 'linear'
  }),
  methods: {
    getRandomHex(){
      return "#" + Math.floor(Math.random() * 16777215).toString(16);
    },
    createChart(){
      try {
          this.loaded = false;
       fetch(`https://api.covid19api.com/dayone/country/${this.country}/status/confirmed`)
        .then(response => response.json())
        .then(summaryData => {
          const filteredSummaryData = summaryData.map(sd => {
              return {
                Date: sd.Date.slice(5,10),
                Confirmed: sd.Cases
              };
            }) 
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
            scales: {
              yAxes:[{
                type: this.scale
              }]
            },
            title: {
              display: false
              },
            legend: {
              display: false,
            },
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
    },
    scale: function(){
      this.createChart();
    }
  }
  ,
  async mounted() {
    try {
      this.loaded = false;
      fetch(`https://api.covid19api.com/countries`)
        .then(response => response.json())
        .then(countries => {

          this.allCountries = countries.sort((a,b)=>{
            if(a.Country<b.Country){
              return -1;
            } else if (a.Country>b.Country){
              return 1;
            } else {
              return 0;
            }
          });
        })
        
    } catch (e) {
      console.error(e);
    }

    

  }
};
</script>

<style>

 
</style>

