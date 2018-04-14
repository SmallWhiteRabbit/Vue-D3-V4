<template>
  <div id="rectDiag"></div>
</template>

<script>
import * as d3 from 'd3'
export default {
  name: 'rectDiag',
  mounted () {
    var width = 800
    var height = 500
    var data = [50, 20, 30, 60, 70, 80, 100, 120]
    var padding = {left: 30, right: 30, top: 30, bottom: 30}
    const svg = d3.select(this.$el).append('svg').attr('width', width).attr('height', height)
    // 坐标尺的范围
    var xScale = d3.scaleBand().domain(d3.range(data.length)).range([0, width - padding.right - padding.left])
    var yScale = d3.scaleLinear().domain([0, d3.max(data)]).range([height - padding.top - padding.bottom, 0])
    // 坐标尺的位置 格数
    var xAxis = d3.axisBottom(xScale).ticks(data.length)
    var yAxis = d3.axisLeft(yScale).ticks(5)
    // 添加坐标
    svg.append('g').attr('class', 'axis').attr('transform', 'translate(' + padding.left + ',' + (height - padding.top) + ' )').call(xAxis)
    svg.append('g').attr('class', 'axis').attr('transform', 'translate(' + padding.left + ',' + padding.top + ' )').call(yAxis)
    svg.selectAll('rect').data(data).enter().append('rect')
      .attr('transform', 'translate(' + padding.left + ',' + 0 + ' )')
      .attr('x', function (d, i) { return xScale(i) })
      .attr('y', function (d) { return height - yScale(d) - padding.top })
      .attr('width', function (d) { return xScale.bandwidth() / 5 })
      .attr('height', function (d) { return yScale(d) + 0 })
      .attr('fill', 'blue')
  }
}
</script>
<style>
</style>
