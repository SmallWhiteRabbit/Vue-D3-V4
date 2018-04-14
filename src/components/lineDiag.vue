<template>
  <div>
    <div class="data">
      <span>用来作图的数据：</span><textarea placeholder="格式如下："  :value="data" @change = "handleChange($event)"></textarea>
    </div>
  </div>
</template>

<script>
import * as d3 from 'd3'
export default {
  name: 'lineDiag',
  data: function () {
    return {
      data: '5, 3, 1, 5, 2, 4, 2',
      xMarks: "'Mon', 'Tues', 'Wed', 'Thur', 'Fri', 'Sat', 'Sun' "
    }
  },
  mounted: function () {
    var data = this.data.split(',')
    console.log(typeof (data))
    //    var xMarks = this.xMarks.split(',')
    //    console.log(typeof (xMarks))
    //    var data = [5, 3, 1, 5, 2, 4, 2]
    //    var xMarks = ['Mon', 'Tues', 'Wed', 'Thur', 'Fri', 'Sat', 'Sun']
    this.drawLineDiag(data)
  },
  methods: {
    handleChange: function (e) {
      this.data = e.target.value.split(',')
      //      this.xMarks = e.target.value.split(',')
      e.target.parentNode.nextSibling.remove()
      this.drawLineDiag(this.data)
    },
    drawLineDiag: function (data) {
      var xMarks = ['Mon', 'Tues', 'Wed', 'Thur', 'Fri', 'Sat', 'Sun']
      var width = 600
      var height = 400
      var title = 'Beijing'
      var subTitle = 'Computure'
      // 定义画布
      const svg = d3.select(this.$el).append('svg').attr('width', width).attr('height', height)
      // 添加标题
      svg.append('g').append('text').text(title).attr('x', 420).attr('y', 80)
      // 添加副标题
      svg.append('g').append('text').text(subTitle).attr('x', 420).attr('y', 100)
      // 横坐标比例尺
      var xScale = d3.scaleLinear().domain([0, data.length - 1]).range([20, 400])
      // 纵坐标比例尺
      var yScale = d3.scaleLinear().domain([d3.max(data), 0]).range([20, 260])
      // 横坐标网格线
      var xLnner = d3.axisBottom(xScale).tickSize(-240, 0, 0).ticks(data.length)
      // 添加网格线
      svg.append('g').attr('class', 'inner_line').attr('transform', 'translate(0,' + 260 + ')').call(xLnner).selectAll('text').text('')
      // 纵轴网格线
      var yLnner = d3.axisLeft(yScale).tickSize(-380, 0, 0).tickFormat('').ticks(5)
      // 添加网格线
      svg.append('g').attr('class', 'inner_line').attr('transform', 'translate(' + 20 + ',0)').call(yLnner)
      // 定义横轴
      var xAxis = d3.axisBottom(xScale).ticks(data.length)
      // 添加横坐标轴
      svg.append('g').attr('class', 'axis').attr('transform', 'translate(0,' + 260 + ')').call(xAxis).selectAll('text').text(function (d) { return xMarks[d] })
      // 定义纵轴
      var yAxis = d3.axisLeft(yScale).ticks(5)
      svg.append('g').attr('class', 'axis').attr('transform', 'translate(' + 20 + ',0)').call(yAxis)
      // 画线
      var line = d3.line().x(function (d, i) { return xScale(i) }).y(function (d) { return yScale(d) })
      svg.append('path').attr('d', line(data)).style('fill', '#F00').style('fill', 'none').style('stroke-width', 1).style('stroke', '#09F').style('stroke-opacity', 0.9)
      // 画点
      svg.selectAll('circle').data(data).enter().append('circle').attr('cx', function (d, i) { return xScale(i) }).attr('cy', function (d) { return yScale(d) }).attr('r', 5).attr('fill', '#09F')
    }
  }

}
</script>
<style>
  body{
    height: 100%;
  }
  .title{font-family:Arial,微软雅黑;font-size:18px;text-anchor:middle;}
  .subTitle{font-family:Arial,宋体;font-size:12px;text-anchor:middle;fill:#666}

  .axis path,
  .axis line {
    fill: none;
    stroke: black;
    shape-rendering: crispEdges;
  }

  .axis text {
    font-family: sans-serif;
    font-size: 11px;
    fill:#999;
  }
  .inner_line path,
  .inner_line line {
    fill: none;
    stroke:#E7E7E7;
    shape-rendering: crispEdges;
  }
  .data{
    position: absolute;
    width: 200px;
    left: 20px;
    text-align: left;
  }
  .data textarea{
    width: 250px;
    height: 80px;}

</style>
