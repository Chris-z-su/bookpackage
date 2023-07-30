<template>
  <div id="app">
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="中国诗词" name="poetry">
        <Poetry></Poetry>
      </el-tab-pane>

      <el-tab-pane label="圣经阅读" name="bible">
        <Bible></Bible>
      </el-tab-pane>

      <el-tab-pane label="学习强国" name="Study">
        <Study></Study>
      </el-tab-pane>

      <el-tab-pane label="接口统计" name="ECharts">
        <TestECharts></TestECharts>
      </el-tab-pane>

      <el-tab-pane label="自定义函数" name="MyChart">
        <MyChart></MyChart>
      </el-tab-pane>

      <el-tab-pane label="其他" name="other">
        其他 TypeWriter
        <TypeWrite></TypeWrite>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import Poetry from './components/Poetry.vue'
import Bible from './components/Bible.vue'
import Study from './components/Study.vue'
import TypeWrite from './components/TypeWrite.vue'
import TestECharts from './components/TestECharts.vue'
import MyChart from './components/MyChart.vue'

export default {
  name: 'App',
  components: {
    Poetry,
    Bible,
    Study,
    TypeWrite,
    TestECharts,
    MyChart
  },
  data: function () {
    return {
      // tab页
      activeName: 'MyChart' // 默认
    }
  },
  created: function () {
    console.log("App组件被创建了");
  },
  mounted: function () {
    console.log("App组件被挂载完毕");
  },
  methods: {
    // 切换tab
    handleClick(tab, event) {
      console.log(tab, event);
    },
  }
}
</script>

<style>

</style>

<!-- 
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
-->
