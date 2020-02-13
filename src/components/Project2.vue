<template>
  <div id="project2" class="container">
      <div class="row make-space">
        <div class="col-md-6">
          <button id="play-button" class="btn btn-primary">Play</button>
          <button id="reset-button" class="btn btn-primary">Reset</button>
        </div>
        <div class="col-md-6">
          <select id="continent-select" class="form-control">
            <option seleted-value="all">All</option>
            <option value="africa">Africa</option>
            <option value="europe">Europe</option>
            <option value="americas">Americas</option>
            <option value="asia">Asia</option>
          </select>
        </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          <div id="chart_area">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as d3 from 'd3'
import d3Tip from 'd3-tip'
import $ from 'jquery'

console.log($)

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
      yearLabel: null,
      flag: false,
      transition: null,
      dataIndex: 0,
      radius: null,
      circleColor: null,
      tip: null,
      interval: null,
      data: null
    }
  },

  methods: {
    step () {
      self.dataIndex++
      if (self.dataIndex >= self.data.length) {
        self.dataIndex = 0
      }

      self.update(self.data[self.dataIndex])
    },

    update (data) {
      let self = this

      let countries = data.countries
      let year = data.year

      self.yearLabel.text(year)

      // data joinning
      let circles = self.g.selectAll('circle')
        .data(countries, function (d) {
          return d.country
        })

      // population scales update
      self.radius.domain(d3.extent(countries.map(c => c.population)))

      // delete old items not in our current data
      circles.exit().remove()

      // Enter new items in our data (
      circles.enter()
        .append('circle')
        .attr('cx', function (d) {
          return self.x(d.income)
        })
        .attr('cy', function (d) {
          return self.y(d.life_exp)
        })
        .attr('r', function (d) {
          return self.radius(d.population)
        })
        .attr('fill', function (d) {
          return self.circleColor(d.continent)
        })
        .on('mouseover', self.tip.show) // before merge since we need to attach the event once
        .on('mouseout', self.tip.hide)
        .merge(circles)
        .transition(self.transition)
        .attr('cx', function (d) {
          return self.x(d.income)
        })
        .attr('cy', function (d) {
          return self.y(d.life_exp)
        })
    }
  },

  mounted () {
    let self = this

    // Initialization
    self.gWidth = self.width - self.margin.left - self.margin.right
    self.gHeight = self.height - self.margin.top - self.margin.bottom

    self.transition = d3.transition().duration(100)
    self.tip = d3Tip().attr('class', 'd3-tip')
      .html(function (d) {
        let text = '<strong>Country</strong> <span style="color:red">' + d.country + '</span><br>'
        text += '<strong>Continent</strong> <span style="color:red;text-transform: capitalize">' + d.continent + '</span><br>'
        text += '<strong>GDP Per Capita</strong> <span style="color:red">' + d3.format('$,.0f')(d.income) + '</span><br>'
        text += '<strong>Life Expectancy</strong> <span style="color:red">' + d3.format('.2f')(d.life_exp) + '</span><br>'
        text += '<strong>Population</strong> <span style="color:red">' + d3.format(',.0f')(d.population) + '</span><br>'
        return text
      })

    self.svg = d3.select('#chart_area')
      .append('svg')
      .attr('width', self.width)
      .attr('height', self.height)

    self.g = self.svg.append('g')
      .attr('width', self.gWidth)
      .attr('height', self.gHeight)
      .attr('transform', 'translate(' + self.margin.left + ', ' + self.margin.top + ')')

    self.g.call(self.tip)

    // define our scales
    self.y = d3.scaleLinear()
      .domain([0, 90])
      .range([self.gHeight, 0])

    self.x = d3.scaleLog()
      .domain([300, 150000])
      .range([0, self.gWidth])
      .base(10)

    // Population scales
    self.radius = d3.scaleSqrt()
      .range([5, 25])

    self.circleColor = d3.scaleOrdinal()
      .domain(['europe', 'asia', 'americas', 'africa'])
      .range(d3.schemeCategory10)

    // define our axes groups
    self.xAxisGroup = self.g.append('g')
      .attr('transform', 'translate(0, ' + self.gHeight + ')')
    self.yAxisGroup = self.g.append('g')

    // defining our axes and attaching them to our groups
    self.xAxisCall = d3.axisBottom(self.x)
      .ticks(3)
      .tickValues([400, 4000, 40000])
      .tickFormat(function (d) {
        return '$' + d
      })
    self.xAxisGroup.call(self.xAxisCall)

    self.yAxisCall = d3.axisLeft(self.y)
    self.yAxisGroup.call(self.yAxisCall)

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

    self.yearLabel = self.g.append('text')
      .attr('font-size', '30px')
      .attr('x', self.gWidth)
      .attr('y', self.gHeight - 10)
      .attr('class', 'customLabel')
      .attr('text-anchor', 'end')

    // our graph legends
    const continents = ['africa', 'americas', 'europe', 'asia']
    let legend = self.g.append('g')
      .attr('transform', 'translate(' + (self.gWidth - 10) + ', ' + (self.gHeight - 130) + ')')

    continents.forEach(function (continent, i) {
      let legendRow = legend.append('g')
        .attr('transform', 'translate(0, ' + (i * 20) + ')')

      // add the color rects
      legendRow.append('rect')
        .attr('width', 10)
        .attr('height', 10)
        .attr('fill', self.circleColor(continent))

      // add the continent name
      legendRow.append('text')
        .attr('x', -10)
        .attr('y', 10)
        .attr('text-anchor', 'end')
        .style('text-transform', 'capitalize')
        .text(continent)
    })

    // new
    d3.json('static/data/data.json')
      .then(function (data) {
        data.forEach(function (item) {
          item.countries = item.countries.filter(d => (d.income != null &&
            d.life_exp != null && d.population != null))
        })
        self.data = data
        let singleYear = self.data[self.dataIndex]
        // first rendering
        self.update(singleYear)
      })
      .catch(function (err) {
        console.log(err)
      })

    $('#play-button')
      .on('click', function () {
        self.interval = setInterval(self.step, 300)
      })
  }
}
</script>

<style>
.make-space {
  margin-bottom: 50px;
}

#project2 {
  padding-top: 100px;
}

.customLabel {
  color: #A9A9A9;
}

.d3-tip {
  line-height: 1;
  font-weight: bold;
  padding: 12px;
  background: rgba(0, 0, 0, 0.8);
  color: #fff;
  border-radius: 2px;
  pointer-events: none;
}

/* Creates a small triangle extender for the tooltip */
.d3-tip:after {
  box-sizing: border-box;
  display: inline;
  font-size: 10px;
  width: 100%;
  line-height: 1;
  color: rgba(0, 0, 0, 0.8);
  position: absolute;
  pointer-events: none;
}

/* Northward tooltips */
.d3-tip.n:after {
  content: "\25BC";
  margin: -1px 0 0 0;
  top: 100%;
  left: 0;
  text-align: center;
}

/* Eastward tooltips */
.d3-tip.e:after {
  content: "\25C0";
  margin: -4px 0 0 0;
  top: 50%;
  left: -8px;
}

/* Southward tooltips */
.d3-tip.s:after {
  content: "\25B2";
  margin: 0 0 1px 0;
  top: -8px;
  left: 0;
  text-align: center;
}

/* Westward tooltips */
.d3-tip.w:after {
  content: "\25B6";
  margin: -4px 0 0 -1px;
  top: 50%;
  left: 100%;
}
</style>
