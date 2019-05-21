<template>
<div>
    <h3>C：动态表格2</h3>
    <span>{{time}}</span>
    <el-row type="flex" justify="center">
      <el-col :span="12">
        <el-table :data="tableData" class="table">
          <el-table-column prop="time" label="时间" width="180"></el-table-column>
          <el-table-column prop="price" label="价格" width="180">
              <template scope="scope">
                        <span v-if="scope.row.side=='buy'" style="color:green" v-text="scope.row.price"></span>
                        <span v-else style="color: red" v-text="scope.row.price"></span>
               </template>
          </el-table-column>
          <el-table-column prop="num" label="数量"></el-table-column>
          <el-table-column prop="sum" label="总计"></el-table-column>
        </el-table>
      </el-col>
    </el-row>
</div>
</template>
<script>
export default {
  name: "partc",
  data() {
    return {
        tableData:[],
        time:''
    };
  },
  created() {
      this.getData()
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
        .post("https://interview.westmatrix.cn/api/v1/dynamic_table2", {})
        .then(res => {
            this.time=this.fdate(res.data.timestamp)
            for (let i = 0; i < 15; i++) {
            let temp = res.data.transactions[i].price * res.data.transactions[i].size;
            this.tableData.push({
              time:this.fdate(res.data.transactions[i].execution_time),
              price: res.data.transactions[i].price,
              num: res.data.transactions[i].size,
              sum: temp.toFixed(3),
              side:res.data.transactions[i].side
            });
          }
        });
    },
    fdate(da) {
      da = new Date(da);
      let hour = da.getHours() + ":";
      let min = da.getMinutes() + ":";
      let sec = da.getSeconds();
      return hour + min + sec;
    }
  }
};
</script>