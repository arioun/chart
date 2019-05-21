<template>
  <div id="app">
    <el-row class="row">
      <el-button size="mini" @click="change(1)">partA</el-button>
      <el-button size="mini" @click="change(2)">partB</el-button>
      <el-button size="mini" @click="change(3)">partC</el-button>
      <el-button size="mini" @click="change(4)">partD</el-button>
      <el-button size="mini" @click="change(5)">partE</el-button>
      </el-row>
      <component :is="com"></component>
  </div>
</template>

<script>
import partA from './components/parta.vue'
import partB from './components/partb.vue'
import partC from './components/partc.vue'
import partD from './components/partd.vue'
import partE from './components/parte.vue'
export default {
  name: 'app',
  components: {
    partA,
    partB,
    partC,
    partD,
    partE
  },
  data() {
    return {
      com:'partA'
    }
  },
  methods: {
    change(com){
      if (com==1) {
        this.com='partA'
      }else if (com==2) {
        this.com='partB'
      }else if (com==3) {
        this.com='partC'
      }else if (com==4) {
        this.com='partD'
      }else if (com==5) {
        this.com='partE'
      }
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
.row{
  margin-bottom: 30px;
}
</style>
