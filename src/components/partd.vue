<template>
  <div>
    <h3>D：简单地图数据显示</h3>
    <div id="mychart" class="chart"></div>
  </div>
</template>
<script>
import "../../node_modules/echarts/map/js/china.js";
export default {
  name: "partd",
  data() {
    return {
      data: [
              {
                name: "成都",
                value: "异常"
              },
              {
                name: "深圳",
                value: "未知"
              },
              {
                name: "北京",
                value: "未知"
              },
              {
                name: "深圳",
                value: "未知"
              },
              {
                name: "上海",
                value: "异常"
              }
            ]
    };
  },
  mounted() {
    this.drawLine();
  },
  created() {
      this.getData()
  },
  methods: {
    drawLine() {
      let myChart = this.$echarts.init(document.getElementById("mychart"));
      myChart.setOption({
        title: [

        ],
        tooltip: {
          show: true,
          formatter: function(params) {
              //console.log(params);
              
            return params.name + "：" + params.value;
          }
        }, // 鼠标移到图里面的浮动提示框
        visualMap: {
          orient: "horizontal",
          type: "continuous",
          itemWidth: 9,
          itemHeight: 55,
          text: ["高", "低"],
          showLabel: true,
          seriesIndex: [0],
          min: 0,
          max: 5,
          inRange: {
            color: ["#e4f2dc", "#cde8bc", "#a3ea77", "#90d764", "#68b837"]
          },
          textStyle: {
            color: "#7B93A7",
            fontSize: 14
          }
        },
        geo: {
          roam: false,
          map: "china",
          label: {
            emphasis: {
              show: false
            }
          },

          itemStyle: {
            normal: {
              areaColor: "#a3ea77",
              borderColor: "#fff"
            },
            emphasis: {
              areaColor: "#68b837"
            }
          },
          regions: [
            {
              name: "南海诸岛",
              value: 0,
              itemStyle: {
                normal: {
                  opacity: 0,
                  label: {
                    show: false
                  }
                }
              }
            },
          ]
        },
        series: [
          {
            type: "map",
            map: "china",
            geoIndex: 0,
            aspectScale: 0.2, //长宽比
            showLegendSymbol: false, // 存在legend时显示
            label: {
              normal: {
                show: false
              },
              emphasis: {
                show: false,
                textStyle: {
                  color: "#fff"
                }
              }
            },
            roam: true,
            itemStyle: {
              normal: {
                areaColor: "#031525",
                borderColor: "#3B5077"
              },
              emphasis: {
                areaColor: "#2B91B7"
              }
            },
            animation: false,
            data: this.data
          }
        ]
      });
    },
    getData() {
      this.$http
        .post("https://interview.westmatrix.cn/api/v1/server_status", {})
        .then(res => {
           console.log(res);
           
        });
    },
  }
};
</script>