<template>
    <section class="chart-container">
        <el-row>
            <el-col :span="12">
                <div id="sankeyChart" style="width:100%; height:400px;"></div>
            </el-col>
        </el-row>
    </section>
</template>

<script>
    import echarts from 'echarts'
    import { getSankeyData } from '../../api/api';
    var sankeyData=require('@/mock/data/product.json')
    export default {
        data() {
            return {
                chartSankeyChart: null,
                data: []
            }
        },

        methods: {
            drawSankeyChart() {
              let para = {
              };

              //NProgress.start();
              // 通过axios 获取桑吉图数据
              getSankeyData(para).then((res) => {
                console.log(res)
                this.data = res.data
                //NProgress.done();
                this.chartColumn = echarts.init(document.getElementById('sankeyChart'));
                this.chartColumn.setOption({
                  title: {
                      text: 'Sankey Diagram'
                  },
                  tooltip: {
                      trigger: 'item',
                      triggerOn: 'mousemove'

                  },
                  series: [
                      {
                          type: 'sankey',
                          layout:'none',
                          data: this.data.nodes,
                          links: this.data.links,
                          itemStyle: {
                              normal: {
                                  borderWidth: 1,
                                  borderColor: '#aaa'
                              }
                          },
                          lineStyle: {
                              normal: {
                                  curveness: 0.5
                              }
                          }
                      }
                  ]
                });
              });
          },
          drawCharts() {
              this.drawSankeyChart()
          },
        },

        mounted: function () {
            this.drawCharts()
        },
        updated: function () {
            this.drawCharts()
        }
    }
</script>

<style scoped>
    .chart-container {
        width: 100%;
        float: left;
    }
    /*.chart div {
        height: 400px;
        float: left;
    }*/

    .el-col {
        padding: 30px 20px;
    }
</style>
