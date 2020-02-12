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
      width: 500,
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
      let value = self.flag ? 'revenue' : 'profit'
      let label = self.flag ? 'Revenue' : 'Profit'

      // adjusting our scales to varying sources of data
      self.x.domain(data.map(d => d.month))
      self.y.domain([0, d3.max(data.map(d => d[value]))])

      // defining our axes and attaching them to our groups
      self.xAxisCall = d3.axisBottom(self.x)
      self.xAxisGroup.transition(self.transition).call(self.xAxisCall)

      self.yAxisCall = d3.axisLeft(self.y)
        .tickFormat(function (d) {
          return d + '$'
        })
      self.yAxisGroup.transition(self.transition).call(self.yAxisCall)

      // updating the label of our y axis
      self.xLabel.text(label)

      // data joinning
      let circles = self.g.selectAll('circle')
        .data(data, function (d) {
          return d.month
        })

      // delete old items not in our current data
      circles.exit()
        .attr('fill', 'red')
        .transition(self.transition)
        .attr('cy', self.y(0))
        .remove()

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
          return self.x(d.month) + self.x.bandwidth() / 2
        })
        .attr('cy', self.y(0))
        .attr('r', 5)
        .attr('fill', 'grey')
        .merge(circles)
        .transition(self.transition)
        .attr('cy', function (d) {
          return self.y(d[value])
        })
        .attr('cx', function (d) {
          return self.x(d.month) + self.x.bandwidth() / 2
        })
    }
  },

  mounted () {
    let self = this

    // Initialization
    self.gWidth = self.width - self.margin.left - self.margin.right
    self.gHeight = self.height - self.margin.top - self.margin.bottom

    self.transition = d3.transition().duration(750)

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

    self.x = d3.scaleBand()
      .range([0, self.gWidth])
      .paddingInner(0.3)
      .paddingOuter(0.3)

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
      .text('Month')

    self.xLabel = self.g.append('text')
      .attr('font-size', '20px')
      .attr('transform', 'rotate(-90)')
      .attr('x', -self.gHeight / 2)
      .attr('y', -60)
      .attr('text-anchor', 'middle')

    // new
    d3.json('static/data/data.json')
      .then(function (data) {
        data.forEach(function (item) {
          console.log(item.countries.filter(d => (d.income != null && d.life_exp != null)))
        })
      })
      .catch(function (err) {
        console.log(err)
      })

    // read the data
    d3.json('static/data/revenue.json')
      .then(function (data) {
        data.forEach(function (d) {
          d.revenue = +d.revenue
          d.profit = +d.profit
        })
        // updating
        d3.interval(function () {
          self.flag = !self.flag
          let newData = self.flag ? data : data.slice(1)
          self.update(newData)
        }, 1000)

        // first time rendering
        self.update(data)
      })
      .catch(function (err) {
        console.log(err)
      })
  }
}
</script>

<style scoped>
#project2 {
  padding-top: 100px;
}
</style>