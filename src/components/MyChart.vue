<template>
    <div>
        <input v-model="formula" placeholder="请输入数学公式" @keyup.enter="updateChart" />
        <!-- <div v-if="isValid">输入的数学函数有效</div>
        <div v-else>输入的数学函数无效</div> -->
        <div id="chartContainer" style="width: 600px; height: 600px;"></div>
    </div>
</template>
  
<script>
import * as echarts from 'echarts';
// import math from 'mathjs';
// import * as math from 'mathjs'; // 导入整个 Math.js 库
import { create, all } from 'mathjs'; // 导入 create 和 all 方法

export default {
    data() {
        return {
            formula: 'x', // 用户输入的函数公式
            isValid: false,
            math: '',
            parsed: ''
        };
    },
    methods: {
        // 使用快捷键“Crtl+shift+Y”隐藏“调试控制台”
        // 更新图表
        updateChart: function () {
            const chart = echarts.init(document.getElementById('chartContainer'));
            const option = {
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
                    min: -200,
                    max: 200,
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
                        endValue: 20
                    }
                ],
                series: [
                    {
                        type: 'line',
                        showSymbol: false,
                        clip: true,
                        data: this.generateData()
                    }
                ],
            };
            chart.setOption(option);
        },
        // 解析函数公式并生成图表数据
        generateData: function () {
            // 校验函数
            this.validateFormula();
            console.log(this.isValid);
            if (!this.isValid) {
                console.log("请重新输入!");
                return;
            }

            let data = [];
            for (let x = -200; x <= 200; x += 0.1) {
                data.push([x, this.func(x)]);
            }
            return data;
        },
        // 参考：https://codeleading.com/article/82506464151/
        // 校验用户输入的数学公式
        validateFormula: function () {
            try {
                console.log(this.formula);

                this.math = create(all);

                // 使用 Math.js 解析和计算用户输入的函数表达式
                this.parsed = this.math.parse(this.formula);
                console.log(this.parsed);

                // 检查是否包含不允许的函数或符号
                // const allowedFunctions = ['sin', 'cos', 'tan', 'log', 'exp', 'sqrt']; // 可以根据需要添加其他允许的函数
                // parsed.traverse(function (node) {
                //     if (node.isSymbolNode && !allowedFunctions.includes(node.name)) {
                //         throw new Error('Invalid function or symbol found');
                //     }
                // });

                // 如果解析和计算成功，表示输入的数学函数有效
                this.isValid = true;
            } catch (error) {
                // 如果解析过程中出现错误，表示输入的数学函数无效
                console.log(error);
                this.isValid = false;
            }
        },
        func: function (x) {
            // 准备计算表达式时需要的作用域（scope）参数
            const scope = {
                x: x, // 设置变量 x 的值为 5
            };

            try {
                // 使用 math.evaluate() 计算表达式并传入作用域参数
                const result = this.parsed.evaluate(scope);
                // console.log(result); // 输出结果为 17（3 * 5 + 2）

                return result;
            } catch (error) {
                // console.error('表达式计算错误：', error);
            }
        },

    },
    mounted() {
        this.updateChart();
    },
};
</script>
  
<style>
/* 在这里可以添加样式 */
</style>
  