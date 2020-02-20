<template>
  <div class="echarts">
    <div :style="{height:'400px',width:'100%'}" ref="myEchart"></div>
  </div>
</template>
<script>
import shantouJson from 'echarts/map/json/citys/440500.json'
import hongkongJson from 'echarts/map/json/citys/810100.json'
import chinaJson from 'echarts/map/china.js'
export default {
  data(){
    return{
      shantouJson,
      chinaJson,
      hongkongJson
    }
  },
  methods:{
    initMap(){
      console.log("shantou",this.shantouJson)
      console.log("china",this.chinaJson)
      console.log("hongkong",this.hongkongJson)
      this.$echarts.registerMap('shantou',this.shantouJson)
      var myChart = this.$echarts.init(document.getElementById("map"))
      myChart.setOption({
        backgroundColor: "#02AFDB",
        title: {
            text: '汕头市各区办件量统计',
          },
        series: [
            {
                name: '汕头市各区办件量',
                type: 'map',
                mapType: 'shantou', // 自定义扩展图表类型
                label: {
                    show: true
                },
                data: [
                    {name: '南澳县', value: 20057},
                    {name: '濠江区', value: 15477},
                    {name: '金平区', value: 31686},
                    {name: '龙湖区', value: 6992},
                    {name: '澄海区', value: 44045},
                    {name: '潮阳区', value: 40689},
                    {name: '潮南区', value: 37659},
                ],
            }
        ]
      })
    }
  },
  mounted(){
    this.initMap();
  }
}
</script>

<style>
.map{
  width: 100%;
  height: 100%;
}
</style> 

