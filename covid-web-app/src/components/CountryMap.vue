<template>
<div>
  <div id="world-map"></div> 
</div>
</template>

<script>
//import * as d3 from 'd3';

export default {
    name:"CountryMap",
    
mounted() {

    var WorldMap = require("world-map");
      WorldMap("world-map", 
      {
        width: 2000,
        height: 1000,
        landsColor: '#008282',
        zoom: false
      });
    }
    
}
</script>

<style scoped>


</style>