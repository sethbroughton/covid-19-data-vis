<template>
  <div id="app">
    <global-cases-chart/>
    <case-by-country/>
  </div>
</template>

<script>
import GlobalCasesChart from './components/GlobalCasesChart.vue'
import CaseByCountry from './components/CaseByCountry.vue'

export default {
  name: 'App',
  components: {
    GlobalCasesChart,
    CaseByCountry
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
