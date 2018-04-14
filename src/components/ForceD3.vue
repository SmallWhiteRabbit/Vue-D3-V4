<template>
    <div id="ForceD3"></div>
</template>

<script>
import * as d3 from 'd3'
let nodes = [
  {name: '桂林'}, {name: '广州'},
  {name: '厦门'}, {name: '杭州'},
  {name: '上海'}, {name: '青岛'},
  {name: '天津'}, {name: '宜昌'}
]
var edges = [
  {source: 0, target: 1}, {source: 0, target: 2},
  {source: 0, target: 3}, {source: 1, target: 4},
  {source: 1, target: 5}, {source: 1, target: 6},
  {source: 1, target: 1}

]
var width = 800
var height = 600
export default {
  name: 'ForceD3',
  mounted () {
    const svg = d3.select(this.$el)
      .append('svg')
      .attr('width', width)
      .attr('height', height)
    let force = d3.forceSimulation(nodes)
      .force('link', d3.forceLink(edges).distance(200))
      .force('charge', d3.forceManyBody())
      .force('center', d3.forceCenter(width / 2, height / 2))
    force.force('link').links(edges)
    force.nodes(nodes).on('tick', ticked)

    var svgEdges = svg.selectAll('.line')
      .data(edges)
      .enter()
      .append('line')
      .style('stroke', '#d65151')
      .style('stroke-width', 3)
      .call(d3.zoom().scaleExtent([-5, 2]))
    let color = d3.scaleOrdinal(d3.schemeCategory20)

    var svgNodes = svg.selectAll('circle')
      .data(nodes)
      .enter()
      .append('circle')
      .attr('cx', function (d) { return d.x })
      .attr('cy', function (d) { return d.y })
      .attr('r', 20)
      .style('fill', function (d, i) { return color(i) })
      .call(d3.drag()
        .on('start', dragstarted)
        .on('drag', dragged)
        .on('end', dragended))
    var svgTexts = svg.selectAll('text')
      .data(nodes)
      .enter()
      .append('text')
      .attr('dx', 20)
      .attr('dy', 8)
      .text(function (d) {
        return d.name
      })
    force.on('tick', function () {
      svgEdges.attr('x1', function (d) { return d.source.x })
        .attr('y1', function (d) { return d.source.y })
        .attr('x2', function (d) { return d.target.x })
        .attr('y2', function (d) { return d.target.y })
      svgNodes.attr('cx', function (d) { return d.x })
        .attr('cy', function (d) { return d.y })
      svgTexts.attr('x', function (d) { return d.x })
        .attr('y', function (d) { return d.y })
    })
    function dragstarted (d) {
      if (!d3.event.active) force.alphaTarget(0.3).restart()
      d.fx = d.x
      d.fy = d.y
    }

    function dragged (d) {
      d.fx = d3.event.x
      d.fy = d3.event.y
    }

    function dragended (d) {
      if (!d3.event.active) force.alphaTarget(0)
      d.fx = null
      d.fy = null
    }
    function ticked () {
      svgEdges.attr('x1', function (d) { return d.source.x })
        .attr('y1', function (d) { return d.source.y })
        .attr('x2', function (d) { return d.target.x })
        .attr('y2', function (d) { return d.target.y })
      svgNodes.attr('cx', function (d) { return d.x })
        .attr('cy', function (d) { return d.y })
      svgTexts.attr('x', function (d) { return d.x })
        .attr('y', function (d) { return d.y })
    }
  },
  data () {
    return {}
  },
  components: {}
}
</script>
<style>

</style>
