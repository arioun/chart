<template>
  <div>
      <el-row type="flex" justify="center">
        <el-col :span="6">
            <el-input type="textarea" :rows="5" placeholder="请输入需要加密的文字" v-model="encryptmsg"></el-input>
        </el-col>
      </el-row>
      <el-row type="flex" justify="center">
        <el-col :span="2">
            <el-button size="mini" @click="encrypt">加密</el-button>
        </el-col>
        <el-col :span="3">
            <i class="el-icon-sort arrow"></i>
        </el-col>

        <el-col :span="2">
            <el-button size="mini" @click="decrypt">解密</el-button>
        </el-col>
      </el-row>
    <el-row type="flex" justify="center">
        <el-col :span="6">
            <el-input type="textarea" :rows="5" placeholder="请输入需要解密的文字" v-model="decryptmsg"></el-input>
        </el-col>
      </el-row>
  </div>
</template>
<script>
export default {
  name: "partre",
  data () {
      return {
          encryptmsg:'',
          decryptmsg:''
      }
  },
  methods: {
      encrypt(){
          if (this.encryptmsg) {
            this.$http.post('https://interview.westmatrix.cn/api/v1/encrypt',{"raw_content":this.encryptmsg}).then(res=>{
              this.decryptmsg=res.data.encrypt_content
              this.encryptmsg=''
          })
          }else{
            this.$message('请输入需要加密的文字');
          }
      },
      decrypt(){
          if (this.decryptmsg) {
            this.$http.post('https://interview.westmatrix.cn/api/v1/decrypt',{"raw_content":this.decryptmsg}).then(res=>{
              this.encryptmsg=res.data.encrypt_content
              this.decryptmsg=''
          })
          }else{
              this.$message('请输入需要解密的文字');
          }
          
      }
  },
};
</script>
<style>
.arrow{
    height: 50px;
    font-size: 50px;
    font-weight: 5px;
}
</style>
