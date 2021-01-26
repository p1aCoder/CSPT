<template>

    <div class="Echarts">
        <div id="main" style="width: 1500px;height:1000px;"></div>
    </div>
</template>

<script>
    export default {
        name: 'Anima',
        data(){
          return{
              nodes:[],
              edges:[],

              links: [ //links是关系，需指明source、target、id
                  {
                      id: 0,
                      source: 0,
                      target: 1
                  },{
                      id: 2,
                      source: 1,
                      target: 2,
                  }
              ],
              nodess: [ //nodes是节点
                  {
                      id: 0,
                      draggable: true,  //可拖拽
                      name: "Myriel",  //节点名称
                      symbolSize: 30  //节点大小，必须指明，否则节点大小为0，不显示图谱
                  },{
                      id: 1,
                      draggable: true,
                      name: "Napoleon",
                      symbolSize: 20
                  },{
                      id: 2,
                      draggable: true,
                      name: "丁酰苯类药物： 氟哌啶醇、螺哌隆等，有可能增强锥体外系症状。 因本药的弱抗多巴胺作用，有可能增强丁酰苯类药物的药理作用。\n" +
                          "钙拮抗剂：尼卡地平、氨氯地平、硝苯吡啶等，有可能增强降压作用。 因本药有5-羟色胺受体介导的中枢性降压作用，有可能增强降压作用。",
                      symbolSize: 10,
                      label: {
                          show: true
                      },
                      itemStyle:{
                          color: '#66ccff',
                      }

                  }
              ]

          }
        },

        methods:{
            myEcharts(){
                // 基于准备好的dom，初始化echarts实例
                var myChart = this.$echarts.init(document.getElementById('main'));

                // 指定图表的配置项和数据
                var categories = [];
                var graph = {  //图谱数据
                    links:this.edges,
                    nodes:this.nodes
                }
                var option = {
                    title: {
                        text: 'CSPT Produced By Anima',
                        subtext: 'Default layout',
                        top: 'bottom',
                        left: 'right'
                    },
                    tooltip: {show:false},
                    legend: [{
                        // selectedMode: 'single',
                        // data: categories.map(function (a) {
                        //     return a.name;
                        // })
                        data:['ad','awf']
                    }],
                    animation: false,
                    series : [
                        {
                            name: 'CSPT',
                            type: 'graph',
                            layout: 'force',
                            data: graph.nodes, //节点
                            links: graph.links, //关系
                            categories: categories,
                            roam: true,
                            label: {
                                position: 'right'
                            },
                            force: {
                                repulsion: 50
                            }
                        }
                    ]
                };

                // 使用刚指定的配置项和数据显示图表。
                myChart.setOption(option);
            }
        },
        mounted() {
            this.myEcharts();
        }
    }
</script>

<style>

</style>
