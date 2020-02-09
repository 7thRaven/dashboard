<template>
    <div>
        <div id="chart"></div>
    </div>
</template>

<script>
/* eslint-disable */
import * as d3 from 'd3'

export default {
  data () {
    return {
      msg: 'Testing'
    }
  },

  mounted () {
    d3.json('static/data/buildings.json').then(function(data){
      data.forEach(function(d){
        d.height = +d.height
      })

      var sorted_data = [...data].sort(function(d1, d2){
        return d1.height < d2.height
      })

      var y = d3.scaleLinear()
        .domain([0, d3.max(sorted_data, d => d.height)])
        .range([0, 400])
      
      console.log(sorted_data.map(d => d.name))
      var x = d3.scaleBand()
        .domain(sorted_data.map(d => d.name))
        .range([0, 400])
        .paddingInner(0.3)
        .paddingOuter(0.3)

      var canvas = d3.select('#chart').append('svg')
        .attr('width', 400)
        .attr('height', 400)

      var bars = canvas.selectAll('rect')
        .data(sorted_data)

      bars.enter().append('rect')
        .attr('x', function(d, i){
          return x(d.name)
        })
        .attr('y', 0)
        .attr('width', x.bandwidth())
        .attr('height', function(d){
          return y(d.height)
        })
        .attr('fill', 'grey')
    })
    .catch(function(err){
      console.log(err)
    })
  }
}
</script>
