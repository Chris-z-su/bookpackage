<template>
    <!-- 为 ECharts 准备一个定义了宽高的 DOM -->
    <div>
        <div id="line" style="width: 600px; height: 400px;">
        
        </div>
        <div id="pie" style="width: 600px; height: 400px;">
        
        </div>
        <div id="func" style="width: 1200px; height: 400px;">
        
        </div>
    </div>
</template>

<script>
// 示例：https://echarts.apache.org/examples/zh/index.html
// https://www.highcharts.com/demo
import * as echarts from 'echarts';

export default{
    name: "ECharts",
    data: function () {
        return {
            // 折线图
            chartOption: {},
            // 饼图
            pieOption: {}
        }
    },
    mounted() {
        this.getLogInfo();
        this.initFunc();
    },
    beforeDestroy() {
        // 销毁图表
        // this.chart.dispose();
    },
    created() {
    },
    methods: {
        initChart: function () {
            // 基于准备好的dom，初始化echarts实例
            var myline = echarts.init(document.getElementById('line'));
            // 指定图表的配置项和数据
            this.lineOption = {
                title: {
                    text: '时间轴',
                    left: 'center',
                },
                xAxis: {
                    type: 'category',
                    data: this.chartOption.xbardata
                },
                yAxis: {
                    type: 'value'
                },
                tooltip : {
                    trigger: 'axis',
                    axisPointer: {
                        type: 'cross',
                        label: {
                            backgroundColor: '#6a7985'
                        }
                    }
                },
                series: [
                    {
                        data: this.chartOption.linedata,
                        type: 'line',
                        label: {
                            show: true
                        }
                    }
                ]
            };
            // 绘制图表
            myline.setOption(this.lineOption);

            var myPie = echarts.init(document.getElementById('pie'));
            this.pieOption = {
                title: {
                    text: '各接口访问情况',
                    subtext: '单位: 次',
                    left: 'center'
                },
                tooltip: {
                    trigger: 'item'
                },
                legend: {
                    orient: 'vertical',
                    left: 'left'
                },
                series: [
                    {
                        name: '接口名称',
                        type: 'pie',
                        radius: '50%',
                        data: this.pieOption,
                        emphasis: {
                            itemStyle: {
                            shadowBlur: 10,
                            shadowOffsetX: 0,
                            shadowColor: 'rgba(0, 0, 0, 0.5)'
                            }
                        }
                    }
                ]
            };
            myPie.setOption(this.pieOption);
        },
        getLogInfo: function () {
            var that = this;

            this.$http.post("/bookpackage/getLogInfo", {}).then(
            function (resp) {
                console.log(resp.data);
                that.chartOption = resp.data.data.chartData;
                that.pieOption = resp.data.data.pieData;
                that.initChart();
            },
            function (err) {
                console.log(err);
            }
            )
        },
        func: function (x) {
            x /= 10;
            return Math.sin(x) * Math.cos(x * 2 + 1) * Math.sin(x * 3 + 2) * 50;
        },
        generateData: function () {
            let data = [];
            for (let i = -200; i <= 200; i += 0.1) {
                data.push([i, this.func(i)]);
            }
            return data;
        },
        initFunc: function () {
            var myFunc = echarts.init(document.getElementById('func'));
            var funcOption = {
                animation: false,
                grid: {
                    top: 40,
                    left: 50,
                    right: 40,
                    bottom: 50
                },
                xAxis: {
                    name: 'x',
                    minorTick: {
                    show: true
                    },
                    minorSplitLine: {
                    show: true
                    }
                },
                yAxis: {
                    name: 'y',
                    min: -100,
                    max: 100,
                    minorTick: {
                    show: true
                    },
                    minorSplitLine: {
                    show: true
                    }
                },
                dataZoom: [
                    {
                    show: true,
                    type: 'inside',
                    filterMode: 'none',
                    xAxisIndex: [0],
                    startValue: -20,
                    endValue: 20
                    },
                    {
                    show: true,
                    type: 'inside',
                    filterMode: 'none',
                    yAxisIndex: [0],
                    startValue: -20,
                    endValue: 45
                    }
                ],
                series: [
                    {
                    type: 'line',
                    showSymbol: false,
                    clip: true,
                    data: this.generateData()
                    }
                ]
            };
            myFunc.setOption(funcOption);
        }
    }
}

</script>

<style>
    #line {
        float:left;
    }
    #pie {
        float:left;
    }
    #func {
        float:left;
    }
</style>