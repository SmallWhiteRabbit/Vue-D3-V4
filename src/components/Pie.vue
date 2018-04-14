 <template>
   <div>
     <div class="data">
       <span>用来作图的数据：</span><textarea placeholder="格式如下：" :value = "pieData" @change = "handleChange($event)"></textarea>
     </div>
 </div>

</template>
<script>
import * as d3 from 'd3'
const width = 500
const height = 500

export default {
  name: 'Pie',
  template: '<div></div>',
  props: {
  },
  data: function () {
    return {
      pieData: this.value || [30, 10, 43, 55, 13]
    }
  },
  mounted: function () {
    this.drawPie(this.pieData)
  },
  methods: {
    handleChange: function (e) {
      this.pieData = e.target.value.split(',')
      e.target.parentNode.nextSibling.remove()
      this.drawPie(this.pieData)
    },
    drawPie: function (data) {
      const svg = d3.select(this.$el).append('svg')
        .attr('width', width)
        .attr('height', height)
        .attr('id', 'svgId')
      let pie = d3.pie()
      let pieData = pie(this.pieData)
      let outerRadius = 150
      let innerRadius = 0
      let arc = d3.arc()
        .outerRadius(outerRadius)
        .innerRadius(innerRadius)
      let arcs = svg.selectAll('g')
        .data(pieData)
        .enter()
        .append('g')
        .attr('transform', 'translate(' + (width / 2) + ',' + (width / 2) + ')')
      let color = d3.scaleOrdinal(d3.schemeCategory10)
      arcs.append('path')
        .attr('fill', function (d, i) { return color(i) })
        .attr('d', function (d) {
          return arc(d)
        })
      arcs.append('text').attr('transform', function (d) {
        return 'translate( -10, 0 )' + 'translate(' + arc.centroid(d) + ')'
      }).attr('text-color', 'middle')
        .text(function (d) {
          return d.data
        })
    }
  },
  computed: {},
  watch: {
  }
}
</script>
<style>
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
