<template>
  <div id="app">
    <div class="tile is-ancestor">
      <div class="tile is-4 box">
        <global-cases-chart/>
      </div>
      <div class="tile is-4 box">
        <case-by-country/>
      </div>
    </div>
    <!-- <country-map/> -->
  </div>
</template>

<script>
import GlobalCasesChart from './components/GlobalCasesChart.vue'
import CaseByCountry from './components/CaseByCountry.vue';
//import CountryMap from './components/CountryMap.vue';
//import CaseDistribution from './components/CaseDistribution.vue';


export default {
  name: 'App',
  components: {
   GlobalCasesChart,
   CaseByCountry,
   //CountryMap,
  // CaseDistribution
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
