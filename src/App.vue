<template>
  <div id="app">
   <div class="direc">
     <a href="javascript:void(0)" @click="toggleTabs('Pie')"> {{first}}</a>
     <a href="javascript:void(0)" @click="toggleTabs('lineDiag')">{{ second }}</a>
     <a href="javascript:void(0)" @click="toggleTabs('rectDiag')">{{ third }}</a>
     <a href="javascript:void(0)" @click="toggleTabs('ForceD3')">{{ forth }}</a>
   </div>
    <!--<div>-->
      <!--<pie :inputValue="msg"></pie>-->
    <!--</div>-->
    <div class="content">
      <first :is="currentView"></first>
    </div>
  </div>
</template>
<script>
import Pie from './components/Pie.vue'
import ForceD3 from './components/ForceD3.vue'
import lineDiag from './components/lineDiag.vue'
import rectDiag from './components/rectDiag.vue'

export default {
  name: 'App',
  template: '<div></div>',
  data () {
    return {
      first: 'D3饼状图',
      second: 'D3折线图',
      third: 'D3柱状图',
      forth: 'D3力导向图',
      fifth: 'MyCharts力导向图',
      currentView: 'Pie',
      msg: '请输入'
    }
  },
  components: {
    Pie,
    ForceD3,
    lineDiag,
    rectDiag
  },
  mounted: {},
  methods: {
    toggleTabs (tabText) {
      this.currentView = tabText
    }
  },
  computed: {},
  watch: {}
}

</script>

<style>
#app{
  width:100%;
  height:100%;
}
  .direc{
    position: relative;
    top: -7px;
    left: -6px;
min-width:700px;
    width: 102%;
    height: 50px;
    padding: 0px;
    margin: 0px;
    background-color: #41aceb;
  }
  .direc a{
    line-height: 30px;
    font-size: 18px;
    display: inline-block;
    margin: 8px 20px;
    text-decoration: none;
    color: #0000ee;
  }
  .content{
    text-align: center;
    margin: 0 auto;
    width: 100%;
  }
svg{
  left: 50px;
  position: relative;
}
</style>
