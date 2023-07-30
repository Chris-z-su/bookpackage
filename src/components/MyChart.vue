<template>
    <div>
        <input v-model="formula" placeholder="请输入数学公式" @keyup.enter.native="parseFormula" />
        <div ref="chartContainer" style="width: 600px; height: 400px;"></div>
    </div>
</template>
  
<script>
import * as echarts from 'echarts';

export default {
    data() {
        return {
            formula: '', // 用户输入的函数公式
            isValid: false,
        };
    },
    methods: {
        // 解析函数公式并生成图表数据
        parseFormula: function() {
            // 校验函数
            this.validateFormula();

            if (!this.isValid) {
                console.log("请重新输入!");
                return;
            }

            // 在这里实现你的函数公式解析逻辑，将解析结果转换为图表数据
            // 这里仅为演示，假设函数是简单的一元二次函数：y = ax^2 + bx + c
            // const a = 1;
            // const b = 2;
            // const c = 1;

            const xData = [];
            const yData = [];
            for (let x = -20; x <= 20; x++) {
                // const y = a * x * x + b * x + c;
                const y = this.formula;
                xData.push(x);
                yData.push(y);
            }
            return { xData, yData };
        },
        // 更新图表
        updateChart: function() {
            const { xData, yData } = this.parseFormula();
            const chart = echarts.init(this.$refs.chartContainer);
            const option = {
                xAxis: {
                    type: 'category',
                    data: xData,
                },
                yAxis: {
                    type: 'value',
                },
                series: [{
                    type: 'line',
                    data: yData,
                }],
            };
            chart.setOption(option);
        },

        // 参考：https://codeleading.com/article/82506464151/
        // 校验用户输入的数学公式
        validateFormula: function() {
            try {
                // 使用 eval() 函数执行用户输入的公式
                const result = eval(this.formula);

                // 判断计算结果是否是数字
                if (typeof result === 'number' && !isNaN(result)) {
                    this.isValid = true;
                } else {
                    this.isValid = false;
                }
            } catch (error) {
                // 如果执行公式过程中出现错误，表示公式无效
                this.isValid = false;
            }

        }

    },
    mounted() {
        this.updateChart();
    },
};
</script>
  
<style>
/* 在这里可以添加样式 */
</style>
  