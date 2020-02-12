<template>
  <div id="project2" class="container">
    <div id="chart_area">
    </div>
  </div>
</template>

<script>
import * as d3 from 'd3'

export default {
  name: 'Project2',
  data () {
    return {
      margin: {left: 100, top: 10, right: 10, bottom: 150},
      width: 1000,
      height: 500,
      svg: null,
      g: null,
      gWidth: null,
      gHeight: null,
      x: null,
      y: null,
      xAxisCall: null,
      yAxisCall: null,
      xAxisGroup: null,
      yAxisGroup: null,
      yLabel: null,
      xLabel: null,
      flag: false,
      transition: null
    }
  },

  methods: {
    update (data) {
      let self = this
      // let value = self.flag ? 'revenue' : 'profit'
      // let label = self.flag ? 'Revenue' : 'Profit'

      let countries = data.countries
      // let year = data.year

      // adjusting our scales to varying sources of data
      self.x.domain([d3.min(countries.map(c => c.income)),
        d3.max(countries.map(c => c.income))])
      self.y.domain([0, d3.max(countries.map(c => c.life_exp))])

      // defining our axes and attaching them to our groups
      self.xAxisCall = d3.axisBottom(self.x)
        .tickValues([400, 4000, 40000])
        .tickFormat(function (d) {
          return '$' + d
        })
      self.xAxisGroup.call(self.xAxisCall)

      self.yAxisCall = d3.axisLeft(self.y)
      self.yAxisGroup.call(self.yAxisCall)

      // data joinning
      let circles = self.g.selectAll('circle')
        .data(countries)

      // delete old items not in our current data
      circles.exit().remove()

      // Update our existing values
      // rects
      //   .transition(self.transition)
      //   .attr('x', function (d) {
      //     return self.x(d.month)
      //   })
      //   .attr('y', function (d) {
      //     return self.y(d[value])
      //   })
      //   .attr('width', self.x.bandwidth())
      //   .attr('height', function (d) {
      //     return self.gHeight - self.y(d[value])
      //   })

      // Enter new items in our data (
      circles.enter()
        .append('circle')
        .attr('cx', function (d) {
          return self.x(d.income)
        })
        .attr('cy', function (d) {
          return self.y(d.life_exp)
        })
        .attr('r', 5)
        .attr('fill', 'grey')
    }
  },

  mounted () {
    let self = this

    // Initialization
    self.gWidth = self.width - self.margin.left - self.margin.right
    self.gHeight = self.height - self.margin.top - self.margin.bottom

    // self.transition = d3.transition().duration(750)

    self.svg = d3.select('#chart_area')
      .append('svg')
      .attr('width', self.width)
      .attr('height', self.height)

    self.g = self.svg.append('g')
      .attr('width', self.gWidth)
      .attr('height', self.gHeight)
      .attr('transform', 'translate(' + self.margin.left + ', ' + self.margin.top + ')')

    // define our scales
    self.y = d3.scaleLinear()
      .range([self.gHeight, 0])

    self.x = d3.scaleLog()
      .range([0, self.gWidth])
      .base(10)

    // define our axes groups
    self.xAxisGroup = self.g.append('g')
      .attr('transform', 'translate(0, ' + self.gHeight + ')')
    self.yAxisGroup = self.g.append('g')

    // adding labels to our axes
    self.yLabel = self.g.append('text')
      .attr('font-size', '20px')
      .attr('x', self.gWidth / 2)
      .attr('y', self.gHeight + 50)
      .attr('text-anchor', 'middle')
      .text('GDP Per Capita ($)')

    self.xLabel = self.g.append('text')
      .attr('font-size', '20px')
      .attr('transform', 'rotate(-90)')
      .attr('x', -self.gHeight / 2)
      .attr('y', -60)
      .attr('text-anchor', 'middle')
      .text('Life Expentancy (Years)')

    // new
    d3.json('static/data/data.json')
      .then(function (data) {
        data.forEach(function (item) {
          item.countries = item.countries.filter(d => (d.income != null &&
            d.life_exp != null && d.population != null))
        })
        let firstYear = data[0]
        // first rendering
        self.update(firstYear)
      })
      .catch(function (err) {
        console.log(err)
      })

    // read the data
    // d3.json('static/data/revenue.json')
    //   .then(function (data) {
    //     data.forEach(function (d) {
    //       d.revenue = +d.revenue
    //       d.profit = +d.profit
    //     })
    //     // updating
    //     d3.interval(function () {
    //       self.flag = !self.flag
    //       let newData = self.flag ? data : data.slice(1)
    //       self.update(newData)
    //     }, 1000)

    //     // first time rendering
    //     self.update(data)
    //   })
    //   .catch(function (err) {
    //     console.log(err)
    //   })
  }
}
</script>

<style scoped>
#project2 {
  padding-top: 100px;
}
</style>
