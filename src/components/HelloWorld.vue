<template>
  <div class="container">
    <div class="top-box">
      <div class="time">2020年2月6日 14:36:51</div>
      <div class="title">义乌市公积金大数据分析</div>
    </div>
    <div class="left-box">
      <div class="data-area">
        <div class="block">
          <div class="value">3218</div>
          <div class="key">申报办件总数</div>
        </div>
        <div class="block">
          <div class="value">3104</div>
          <div class="key">工作时间办件</div>
        </div>
        <div class="block">
          <div class="value">114</div>
          <div class="key">非工作时间办件</div>
        </div>
      </div>
      <div class="data-area">
        <div class="block">
          <div class="value">1525</div>
          <div class="key">网上申报总数</div>
        </div>
        <div class="block">
          <div class="value">47.39%</div>
          <div class="key">网上申报率</div>
        </div>
        <div class="block">
          <div class="value">38.78%</div>
          <div class="key">网上办结率</div>
        </div>
      </div>
      <div class="data-area">
        <div class="small-block1">电脑端 562</div>
        <div class="small-block2">移动端 208</div>
        <div class="small-block3">窗口端 1332</div>
      </div>
      <div class="lineChart" id="lineChart" ref="lineChart"></div>
      <div class="columnChart" id="columnChart" ref="columnChart"></div>
    </div>
    <div class="middle-box">
      <div class="echarts" ref="myEchart"></div>
    </div>
    <div class="right-box">
      <div class="com-button">
        <button>今日</button>
        <button>昨日</button>
        <button>近7天</button>
        <button>近30天</button>
        <button>总计</button>
      </div>
      <div class="block1">
        <div class="title">调用数据TOP5</div>
        <div class="bigData">
          <div class="w1">调用数据（次）</div>
          <div class="w2">1</div>
          <div class="w2">1</div>
          <div class="w2">0</div>
          <div class="w2">8</div>
          <div class="w2">4</div>
          <div class="w2">5</div>
        </div>
        <div class="barData">
          <div class="barName">义乌市人社局一窗受理</div>
          <div class="bar1"></div>
        </div>
        <div class="barData">
          <div class="barName">义乌人口手机号码查询</div>
          <div class="bar2"></div>
        </div>
        <div class="barData">
          <div class="barName">省内人口信息查询</div>
          <div class="bar3"></div>
        </div>
        <div class="barData">
          <div class="barName">义乌公积金单位个人</div>
          <div class="bar4"></div>
        </div>
      </div>
      <div class="block2">
        <div class="title">数据中心</div>
        <div class="pieChart" id="pieChart" ref="pieChart"></div>
      </div>
    </div>
  </div>
</template>
<script>
import echarts from "echarts";
//   import '../../node_modules/echarts/map/js/world.js'
import shantouJson from "echarts/map/json/citys/440500.json";
import yiwuJson from "echarts/map/json/citys/330782.json";
export default {
  name: "echarts",
  data() {
    return {
      chart: null,
      shantouJson,
      yiwuJson
    };
  },
  mounted() {
    this.mapConfigure();
    this.drawLineChart();
    this.drawColumnChart();
    this.drawPieChart();
  },
  methods: {
    mapConfigure() {
      var geoCoordMap = {
        //可以在地图上显示的城市的坐标信息
        赤岸镇: [120.0174, 29.1114],
        上溪镇: [119.8952, 29.3092],
        义亭镇: [119.957, 29.2217],
        佛堂镇: [120.0243, 29.1989],
        城西街道: [119.9776, 29.3092],
        稠江街道: [120.0201, 29.2732],
        江东街道: [120.1122, 29.2792],
        北苑街道: [120.0517, 29.3307],
        大陈镇: [120.1808, 29.4814],
        后宅街道: [120.0421, 29.3977],
        稠城街道: [120.0943, 29.3235],
        廿三里街道: [120.2055, 29.3499],
        苏溪街道: [120.1781, 29.4205],
        福田街道: [120.1424, 29.3702]
      };
      var HFData = [
        // 数据中name的城市名称必须与geoCoordMap中城市名称一致, 不然关联不上
        [{ name: "赤岸镇" }, { name: "稠城街道", value: 2007 }],
        [{ name: "赤岸镇" }, { name: "赤岸镇", value: 2007 }],
        [{ name: "上溪镇" }, { name: "稠城街道", value: 2007 }],
        [{ name: "上溪镇" }, { name: "上溪镇", value: 2007 }],
        [{ name: "义亭镇" }, { name: "稠城街道", value: 2007 }],
        [{ name: "义亭镇" }, { name: "义亭镇", value: 2007 }],
        [{ name: "佛堂镇" }, { name: "稠城街道", value: 2007 }],
        [{ name: "佛堂镇" }, { name: "佛堂镇", value: 2007 }],
        [{ name: "城西街道" }, { name: "稠城街道", value: 2007 }],
        [{ name: "城西街道" }, { name: "城西街道", value: 2007 }],
        [{ name: "稠江街道" }, { name: "稠城街道", value: 2007 }],
        [{ name: "稠江街道" }, { name: "稠江街道", value: 2007 }],
        [{ name: "江东街道" }, { name: "稠城街道", value: 2007 }],
        [{ name: "江东街道" }, { name: "江东街道", value: 2007 }],
        [{ name: "北苑街道" }, { name: "稠城街道", value: 2007 }],
        [{ name: "北苑街道" }, { name: "北苑街道", value: 2007 }],
        [{ name: "大陈镇" }, { name: "稠城街道", value: 2007 }],
        [{ name: "大陈镇" }, { name: "大陈镇", value: 2007 }],
        [{ name: "后宅街道" }, { name: "稠城街道", value: 2007 }],
        [{ name: "后宅街道" }, { name: "后宅街道", value: 2007 }],
        [{ name: "廿三里街道" }, { name: "稠城街道", value: 2007 }],
        [{ name: "廿三里街道" }, { name: "廿三里街道", value: 2007 }],
        [{ name: "苏溪街道" }, { name: "稠城街道", value: 2007 }],
        [{ name: "苏溪街道" }, { name: "苏溪街道", value: 2007 }],
        [{ name: "福田街道" }, { name: "稠城街道", value: 2007 }],
        [{ name: "福田街道" }, { name: "福田街道", value: 2007 }]
      ];
      var planePath = "arrow"; // 箭头的svg
      // push进去线路开始-结束地点-经纬度
      var convertData = function(data) {
        var res = [];
        for (var i = 0; i < data.length; i++) {
          var dataItem = data[i];
          var fromCoord = geoCoordMap[dataItem[0].name]; //出发地坐标获取
          var toCoord = geoCoordMap[dataItem[1].name]; //目的地坐标获取
          if (fromCoord && toCoord) {
            res.push({
              fromName: dataItem[0].name, //出发地名称
              toName: dataItem[1].name, //目的地名称
              numValue: dataItem[1].value, //数值
              coords: [fromCoord, toCoord]
            });
          }
        }
        return res;
      };
      var max = 0;
      var len = HFData.length;
      for (var i = 0; i < len; i++) {
        if (HFData[i][1].value > max) {
          max = HFData[i][1].value;
        }
      }
      var avg = 0;
      avg = max / 3;
      // var color = ['#FFDF31']; //圆圈和字的颜色，线的颜色，箭头颜色
      var color1 = ["#22BB22", "#FFE153", "#D87A80"]; //绿  黄  红
      // 数据
      var series = [];
      
      // 遍历由合肥到其他城市的线路
        // 配置
        series.push(
          {
            type: "lines",
            zlevel: 1, // 用于 Canvas 分层，不同zlevel值的图形会放置在不同的 Canvas 中
            // effect出发到目的地 的白色尾巴线条
            // 线特效的配置
            effect: {
              show: true,
              period: 6, // 特效动画的时间，单位为 s
              trailLength: 0.1, // 特效尾迹的长度。取从 0 到 1 的值，数值越大尾迹越长
              color: "#46bee9", // 移动箭头拖尾颜色
              /* color: function(params){
                        var num = params;
                        if(num>0&&num<=avg){
                            return color1[2];
                        }else if(num>avg&&num<=2*avg){
                            return color1[1];
                        }else if(num>2*avg&&num<=3*avg){
                            return color1[1];
                        }
                    },*/
              symbol: planePath,
              symbolSize: 6 // 特效标记的大小
            },
            // lineStyle出发到目的地 的线条颜色

            lineStyle: {
              normal: {
                color: function(params) {
                  var num = params.data.numValue;
                  //alert(2*avg);
                  if (num > 0 && num <= avg) {
                    return color1[0];
                  } else if (num > avg && num <= 2 * avg) {
                    return color1[1];
                  } else if (num > 2 * avg && num <= 3 * avg) {
                    return color1[2];
                  }
                },
                width: 1,
                curveness: 0.2
              }
            },
            data: convertData(HFData).numValue //开始到结束数据
          },
          {
            //出发地信息
            type: "lines",
            zlevel: 2,
            effect: {
              show: true,
              period: 6,
              trailLength: 0,
              symbol: planePath,
              symbolSize: 6
            },
            itemStyle: {
              normal: {
                borderWidth: 1,
                lineStyle: {
                  type: "solid",
                  shadowBlur: 10
                }
              }
            },
            lineStyle: {
              normal: {
                color: function(params) {
                  var num = params.data.numValue;
                  if (num > 0 && num <= avg) {
                    return color1[0];
                  } else if (num > avg && num <= 2 * avg) {
                    return color1[1];
                  } else if (num > 2 * avg && num <= 3 * avg) {
                    return color1[2];
                  }
                },
                width: 3,
                opacity: 0.9,
                curveness: 0.2
              }
            },
            data: convertData(HFData)
          },
          {
            // 目的地信息
            type: "effectScatter",
            coordinateSystem: "geo",
            zlevel: 2,
            rippleEffect: {
              brushType: "stroke"
            },
            label: {
              normal: {
                show: true,
                position: "right",
                formatter: "{b}"
              }
            },
            /*symbolSize: function(val) {
                    return val[2]/ 8;
                },*/

            itemStyle: {
              normal: {
                color: function(params) {
                  var str = params.value;
                  var num = str[2];
                  if (params.name == "稠城街道") {
                    return "#F7F7F7";
                  } else {
                    if (num > 0 && num <= avg) {
                      return color1[0];
                    } else if (num > avg && num <= 2 * avg) {
                      return color1[1];
                    } else if (num > 2 * avg && num <= 3 * avg) {
                      return color1[2];
                    }
                  }
                }
              }
            },
            data: HFData.map(function(dataItem) {
              return {
                name: dataItem[1].name,
                value: geoCoordMap[dataItem[1].name].concat([dataItem[1].value])
              };
            })
          },
          {
            name: "办件数", // 浮动框的标题
            type: "map",
            geoIndex: 0,
            // data: [
            //   { name: "南澳县", value: 2007 },
            //   { name: "濠江区", value: 1477 },
            //   { name: "金平区", value: 31686 },
            //   { name: "龙湖区", value: 6992 },
            //   { name: "澄海区", value: 44045 },
            //   { name: "潮阳区", value: 4089 },
            //   { name: "潮南区", value: 37659 }
            // ],
            data: [
              { name: "赤岸镇", value: 2007 },
              { name: "上溪镇", value: 1477 },
              { name: "义亭镇", value: 31686 },
              { name: "佛堂镇", value: 6992 },
              { name: "城西街道", value: 44045 },
              { name: "稠江街道", value: 4089 },
              { name: "江东街道", value: 37659 },
              { name: "北苑街道", value: 2007 },
              { name: "大陈镇", value: 1477 },
              { name: "后宅街道", value: 31686 },
              { name: "稠城街道", value: 69920 },
              { name: "廿三里街道", value: 44045 },
              { name: "苏溪街道", value: 4089 },
              { name: "福田街道", value: 4089 },
            ]
          }
        );

      echarts.registerMap("yiwu", this.yiwuJson);
      let myChart = echarts.init(this.$refs.myEchart); //这里是为了获得容器所在位置
      // window.onresize = myChart.resize;
      myChart.setOption({
        // 进行相关配置
        // title: {
        //   text: "义乌市各区办件数统计",
        //   textStyle: {
        //     color: '#fff',
        //     fontSize: 20
        //   }
        // },
        // backgroundColor: "#02AFDB",
        tooltip: {}, // 鼠标移到图里面的浮动提示框
        visualMap: {
          min: 0,
          max: 50000,
          left: 40,
          top: 20,
          text: ["高", "低"],
          textStyle: {
            color: "#fff"
          },
          realtime: false,
          calculable: true,
          inRange: {
            color: ["#2BBBFF", "#077BDC", "#0045BE", "#03367F"]
          }
        },
        geo: {
          // 这个是重点配置区
          map: "yiwu",
          zoom: 1.3,
          top: 120,
          left: 190,
          label: {
            normal: {
              show: true, // 是否显示对应地名
              textStyle: {
                color: "#fff"
              },
              position: "inside"
            }
          },
          itemStyle: {
            normal: {
              borderColor: "rgba(0, 0, 0, 0.2)"
            },
            emphasis: {
              areaColor: null,
              shadowOffsetX: 0,
              shadowOffsetY: 0,
              shadowBlur: 20,
              borderWidth: 0,
              shadowColor: "rgba(0, 0, 0, 0.5)"
            }
          }
        },
        series: series
        // [
        //   {
        //     type: "scatter",
        //     coordinateSystem: "geo" // 对应上方配置
        //   },
        //   {
        //     name: "办件数", // 浮动框的标题
        //     type: "map",
        //     geoIndex: 0,
        //     // data: [
        //     //   { name: "南澳县", value: 2007 },
        //     //   { name: "濠江区", value: 1477 },
        //     //   { name: "金平区", value: 31686 },
        //     //   { name: "龙湖区", value: 6992 },
        //     //   { name: "澄海区", value: 44045 },
        //     //   { name: "潮阳区", value: 4089 },
        //     //   { name: "潮南区", value: 37659 }
        //     // ],
        //     data: [
        //       { name: "赤岸镇", value: 2007 },
        //       { name: "上溪镇", value: 1477 },
        //       { name: "义亭镇", value: 31686 },
        //       { name: "佛堂镇", value: 6992 },
        //       { name: "城西街道", value: 44045 },
        //       { name: "稠江街道", value: 4089 },
        //       { name: "江东街道", value: 37659 },
        //       { name: "北苑街道", value: 2007 },
        //       { name: "大陈镇", value: 1477 },
        //       { name: "后宅街道", value: 31686 },
        //       { name: "稠城街道", value: 69920 },
        //       { name: "廿三里街道", value: 44045 },
        //       { name: "苏溪街道", value: 4089 },
        //       { name: "福田街道", value: 4089 },
        //     ]
        //   }
        // ]
      });
    },
    drawLineChart() {
      let myChart1 = echarts.init(document.getElementById("lineChart"));
      myChart1.setOption({
        title: {
          text: "申报趋势",
          textStyle: {
            color: "#fff",
            fontSize: 20
          }
        },
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross",
            label: {
              backgroundColor: "#6a7985"
            }
          }
        },
        legend: {
          data: ["示例一", "示例二", "示例三", "示例四"],
          textStyle: {
            color: "#fff"
          }
        },
        toolbox: {
          feature: {
            saveAsImage: {}
          }
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true
        },
        xAxis: [
          {
            type: "category",
            boundaryGap: false,
            axisLine: {
              lineStyle: {
                color: "#fff"
              }
            },
            data: [
              {
                value: "周一",
                textStyle: {
                  color: "#fff"
                }
              },
              {
                value: "周二",
                textStyle: {
                  color: "#fff"
                }
              },
              {
                value: "周三",
                textStyle: {
                  color: "#fff"
                }
              },
              {
                value: "周四",
                textStyle: {
                  color: "#fff"
                }
              },
              {
                value: "周五",
                textStyle: {
                  color: "#fff"
                }
              },
              {
                value: "周六",
                textStyle: {
                  color: "#fff"
                }
              },
              {
                value: "周日",
                textStyle: {
                  color: "#fff"
                }
              }
            ]
          }
        ],
        yAxis: [
          {
            type: "value",
            axisLabel: { color: "#fff" },
            axisLine: {
              lineStyle: {
                color: "#fff"
              }
            },
            splitLine: {
              show: false
            }
          }
        ],
        series: [
          {
            name: "示例一",
            type: "line",
            stack: "总量",
            areaStyle: {},
            data: [120, 132, 101, 134, 90, 230, 210]
          },
          {
            name: "示例二",
            type: "line",
            stack: "总量",
            areaStyle: {},
            data: [220, 182, 191, 234, 290, 330, 310]
          },
          {
            name: "示例三",
            type: "line",
            stack: "总量",
            areaStyle: {},
            data: [150, 232, 201, 154, 190, 330, 410]
          },
          {
            name: "示例四",
            type: "line",
            stack: "总量",
            areaStyle: {},
            data: [320, 332, 301, 334, 390, 330, 320]
          }
        ]
      });
    },
    drawColumnChart() {
      let myChart1 = echarts.init(document.getElementById("columnChart"));
      myChart1.setOption({
        title: {
          text: "窗口运行",
          textStyle: {
            color: "#fff",
            fontSize: 20
          }
        },
        xAxis: {
          type: "category",
          axisLine: {
            lineStyle: {
              color: "#fff"
            }
          },
          data: [
            {
              value: "周一",
              textStyle: {
                color: "#fff"
              }
            },
            {
              value: "周二",
              textStyle: {
                color: "#fff"
              }
            },
            {
              value: "周三",
              textStyle: {
                color: "#fff"
              }
            },
            {
              value: "周四",
              textStyle: {
                color: "#fff"
              }
            },
            {
              value: "周五",
              textStyle: {
                color: "#fff"
              }
            },
            {
              value: "周六",
              textStyle: {
                color: "#fff"
              }
            },
            {
              value: "周日",
              textStyle: {
                color: "#fff"
              }
            }
          ]
        },
        yAxis: {
          type: "value",
          axisLabel: { color: "#fff" },
          axisLine: {
            lineStyle: {
              color: "#fff"
            }
          },
          splitLine: {
            show: false
          }
        },
        series: [
          {
            data: [120, 200, 150, 80, 70, 110, 130],
            type: "bar",
            color: "#0098B0",
            barWidth: 30
          }
        ]
      });
    },
    drawPieChart() {
      let myChart1 = echarts.init(document.getElementById("pieChart"));
      myChart1.setOption({
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: 30,
          top: 30,
          data: ["共享接口", "今日调用", "累计调用"],
          textStyle: {
            color: "#fff",
            fontSize: 20
          }
        },
        series: [
          {
            name: "调用次数",
            type: "pie",
            radius: "55%",
            center: ["50%", "50%"],
            data: [
              { value: 335, name: "共享接口" },
              { value: 310, name: "今日调用" },
              { value: 234, name: "累计调用" }
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            },
            label: {
              color: "#fff",
              fontSize: 20
            },
            labelLine: {
              lineStyle: {
                color: "#fff"
              },
              smooth: 0.2,
              length: 20,
              length2: 30
            }
          }
        ]
      });
    }
  }
};
</script>
<style scoped lang="less">
.container {
  background-color: #04284c;
  width: 2430px;
  height: 1050px;
  padding: 0 1% 0 1%;
}
.top-box {
  width: 100%;
  height: 9%;
  .time {
    color: #fff;
    float: left;
    font-size: 1.3em;
    padding: 1.7% 0 0 0.5%;
  }
  .title {
    color: #fff;
    font-size: 2.5em;
    font-family: PingFangSC-Regular;
    padding: 1% 0 0 41%;
    text-align: left;
  }
}
.left-box {
  width: 33%;
  height: 90%;
  float: left;
  .data-area {
    display: flex;
    flex-direction: row;
    .block {
      width: 100%;
      height: 100px;
      margin: 1%;
      box-shadow: 0 2px 4px 0 rgba(203, 217, 228, 0.6);
      background-color: #01589f;
      .value {
        color: #f8d88d;
        font-size: 2.2em;
        font-weight: 600;
        text-align: center;
        padding: 9% 0 0 0;
      }
      .key {
        color: #fff;
        font-size: 1.2em;
        text-align: center;
        padding: 1% 0 0 0;
      }
    }
    .small-block1 {
      width: 100%;
      height: 35px;
      margin: 1%;
      box-shadow: 0 2px 4px 0 rgba(203, 217, 228, 0.6);
      background-color: #01589f;
      color: #f8d88d;
      font-size: 1.2em;
      font-weight: 500;
      text-align: center;
      padding: 1.5% 0 0 0;
    }
    .small-block2 {
      width: 100%;
      height: 35px;
      margin: 1%;
      box-shadow: 0 2px 4px 0 rgba(203, 217, 228, 0.6);
      background-color: #01589f;
      color: #5abaf2;
      font-size: 1.2em;
      font-weight: 500;
      text-align: center;
      padding: 1.5% 0 0 0;
    }
    .small-block3 {
      width: 100%;
      height: 35px;
      margin: 1%;
      box-shadow: 0 2px 4px 0 rgba(203, 217, 228, 0.6);
      background-color: #01589f;
      color: #51d7ab;
      font-size: 1.2em;
      font-weight: 500;
      text-align: center;
      padding: 1.5% 0 0 0;
    }
  }
  .lineChart {
    width: 99%;
    height: 300px;
    margin: 2% 0 0 0;
    background-color: rgba(3, 51, 95, 0.3);
    border-style: solid;
    border-width: 1px;
    border-color: rgb(3, 51, 95, 1);
  }
  .columnChart {
    width: 99%;
    height: 300px;
    margin: 2% 0 0 0;
    background-color: rgba(3, 51, 95, 0.3);
    border-style: solid;
    border-width: 1px;
    border-color: rgb(3, 51, 95, 1);
  }
}
.middle-box {
  width: 33%;
  height: 90%;
  float: left;
}
.right-box {
  width: 33%;
  height: 90%;
  float: right;
  .com-button {
    display: flex;
    button {
      background-color: #05427b; /* Green */
      border: none;
      color: #93cdfc;
      padding: 15px 32px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      margin: 0 0 0 1%;
    }
  }
  .block1 {
    width: 99%;
    height: 300px;
    margin: 2% 0 0 0;
    background-color: rgba(3, 51, 95, 0.3);
    border-style: solid;
    border-width: 1px;
    border-color: rgb(3, 51, 95, 1);
    .title {
      float: left;
      text-align: left;
      color: #fff;
      font-size: 1.4em;
      font-weight: 600;
      padding: 1% 0 0 1%;
    }
    .bigData {
      height: 40%;
      .w1 {
        width: 20%;
        height: 30%;
        float: left;
        color: #fff;
        font-size: 1.4em;
        padding: 8% 0 0 8%;
      }
      .w2 {
        width: 5%;
        height: 38%;
        font-size: 2em;
        color: #49deff;
        margin: 5% 0 0 1.5%;
        padding: 2% 0 0 0;
        float: left;
        background-color: #0354aa;
        border-style: solid;
        border-width: 5px;
        border-radius: 15px;
        border-color: #fff;
      }
    }

    .barData {
      width: 100%;
      height: 20px;

      margin: 2% 0 0 7%;

      .barName {
        width: 36%;
        color: #fff;
        font-size: 1.3em;
        text-align: left;
        float: left;
      }
      .bar1 {
        width: 40%;
        height: 20px;
        background-color: #046b8a;
        float: left;
      }
      .bar2 {
        width: 30%;
        height: 20px;
        background-color: #046b8a;
        float: left;
      }
      .bar3 {
        width: 20%;
        height: 20px;
        background-color: #046b8a;
        float: left;
      }
      .bar4 {
        width: 10%;
        height: 20px;
        background-color: #046b8a;
        float: left;
      }
    }
  }
  .block2 {
    width: 99%;
    height: 500px;
    margin: 2% 0 0 0;
    background-color: rgba(3, 51, 95, 0.3);
    border-style: solid;
    border-width: 1px;
    border-color: rgb(3, 51, 95, 1);
    .title {
      text-align: left;
      color: #fff;
      font-size: 1.4em;
      font-weight: 600;
      padding: 1% 0 0 1%;
    }
    .pieChart {
      width: 100%;
      height: 100%;
    }
  }
}
.echarts {
  height: 100%;
  width: 100%;
}
</style>