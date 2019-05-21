<template>
  <div class="hello">
    <h3>A：基于时间的数据曲线</h3>
    <el-row type="flex" justify="start" class="a-row">
      <el-col :span="10">
        <el-button size="mini" @click="change('one_day')">天</el-button>
      <el-button size="mini" @click="change('one_week')">周</el-button>
      <el-button size="mini" @click="change('one_month')">月</el-button>
      <el-button size="mini" @click="change('three_months')">3月</el-button>
      <el-button size="mini" @click="change('one_year')">1年</el-button>
      <el-button size="mini" @click="change('this_year')">今年</el-button>
      <el-button size="mini" @click="change('all')">所有</el-button>
      </el-col>
      <el-col :span="1">从</el-col>
      <el-col :span="3">
        <el-input v-model="from"></el-input>
      </el-col>
      <el-col :span="1">到</el-col>
      <el-col :span="3">
        <el-input v-model="to" ></el-input>
      </el-col>
    </el-row>

    <div id="mychart" class="chart"></div>
  </div>
</template>

<script>
import { setInterval } from 'timers';
export default {
  name: "partA",
  data () {
    return {
      type:'one_day',
      from:'',
      to:'',
      times:[],
    }
  },
  mounted() {
    this.drawLine()
    setInterval(() => {
      this.getData()
    }, 60000);
  },
  created () {
    this.getData()
  },
  methods: {
    drawLine() {
      let myChart = this.$echarts.init(document.getElementById("mychart"));
      myChart.setOption({
        xAxis: {
        type: 'time',
        
    },
    yAxis: {
        type: 'value'
    },
    series: [{
        data: [],
        type: 'line'
    }]
      });
    },
    getData(){
      this.$http.post('https://interview.westmatrix.cn/api/v1/price_time_series',
      {"type":this.type}).then(res=>{
        this.from=this.fdate(res.data.from_date);
        this.to=this.fdate(res.data.end_date);
        for (let i = 0; i<res.data.market_line.length; i++) {
          this.times[i]=[res.data.market_line[i].timestamp,res.data.market_line[i].price]
        }
        let myChart = this.$echarts.init(document.getElementById("mychart"));
        myChart.setOption({
          series: [{
          data: this.times,
          type: 'line'
          }]
        })
        this.times=[]
      })
    },
    change(v){
      this.type=v;
      this.getData()
    },
    fdate(da){
    da = new Date(da);
    let year = da.getFullYear()+'年';
    let month = da.getMonth()+1+'月';
    let date = da.getDate()+'日';
    return year+month+date
    }
  }
};
</script>
<style>
.chart {
  width: 1000px;
  height: 800px;
  margin: 0 auto;
}
.input{
  display: inline-block!important;
}
.input input{
  display: inline-block;
  width: 100px;
}
.a-row{
  margin-top: 10px;
}
</style>
