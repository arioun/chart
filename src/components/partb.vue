<template>
  <div>
      <h3>B:动态表格1</h3>
    <span>{{time}}</span>
    <el-row type="flex" justify="center">
      <el-col :span="12">
        <el-table :data="tableData" class="table">
          <el-table-column prop="pos" label="位置" width="180"></el-table-column>
          <el-table-column prop="price" label="价格" width="180"></el-table-column>
          <el-table-column prop="num" label="数量"></el-table-column>
          <el-table-column prop="sum" label="总计"></el-table-column>
        </el-table>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  name: "partb",
  data() {
    return {
      tableData: [],
      time:''
    };
  },
  created() {
    this.getData();
  },
  mounted() {
    setInterval(() => {
      this.tableData = [];
      this.getData();
    }, 5000);
  },
  methods: {
    getData() {
      this.$http
        .post("https://interview.westmatrix.cn/api/v1/dynamic_table1", {})
        .then(res => {
            this.time=this.fdate(res.data.timestamp)
          for (let i = 0; i < res.data.buy.length; i++) {
            let temp = res.data.buy[i].price * res.data.buy[i].size;
            let l = i + 1;
            this.tableData.push({
              pos: "买" + l,
              price: res.data.buy[i].price,
              num: res.data.buy[i].size,
              sum: temp.toFixed(3)
            });
          }

          for (let i = 0; i < res.data.sell.length; i++) {
            let temp = res.data.sell[i].price * res.data.sell[i].size;
            let l = i + 1;
            this.tableData.push({
              pos: "卖" + l,
              price: res.data.sell[i].price,
              num: res.data.sell[i].size,
              sum: temp.toFixed(3)
            });
          }
        });
    },
    fdate(da){
    da = new Date(da);
    let year = da.getFullYear()+'年';
    let month = da.getMonth()+1+'月';
    let date = da.getDate()+'日';
    let hour=da.getHours()+':';
    let min=da.getMinutes()+':'
    let sec=da.getSeconds()
    return year+month+date+hour+min+sec
    }
  }
};
</script>
<style>
</style>

