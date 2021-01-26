<template>
    <el-container>
        <el-header style="z-index: 1;height: 80px;margin-left: -20px;margin-top: -60px">
            <el-card class="login-container" style="background-color: slategrey">
                <p style="font-size: 30px;font-family: 华光行楷_CNKI;color: #eaeaea"><i class="el-icon-bell"></i>药物警戒术语搜集系统 (Collection System of Pharmacovigilance Term)</p>
                <el-row style="display: flex;justify-content: center;margin-bottom: 30px">
                    <p style="font-family: 华光淡古印_CNKI;padding-right: 500px;color: #eaeaea">正在浏览:{{disease}}</p>
                    <SearchBar style="position: absolute;left: 600px" @onSearch="searchResult" ref="searchBar"/>
                </el-row>
<!--                <el-button @click="onClick">test</el-button>-->
<!--                <p>{{test}}</p>-->
            </el-card>
        </el-header>
        <el-container style="margin-top: 10px">
            <el-aside style="width: 250px;height: 112%">
                <admin-menu @indexSelect="handSelect" ref="adminMenu"></admin-menu>
            </el-aside>
            <el-main>
                <Anima ref="anima"/>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
    import AdminMenu from "@/components/AdminMenu";
    import Anima from "@/components/Anima";
    import SearchBar from "@/components/SearchBar";
    export default {
        name: "AdminIndex",
        components: {SearchBar, Anima, AdminMenu},
        data(){
            return{
                disease:'未知',
                test:""
            }
        },
        methods:{
            // onClick(){
            //     this.$axios.get('/message/'+'mouyao').then(resp=>{
            //         if(resp.data.code===200){
            //             // console.log(resp.data)
            //             this.$refs.anima.edges=[]
            //             this.$refs.anima.nodes=[]
            //             this.preprocess(resp.data.result['nodes'],resp.data.result['edges'])
            //             this.$refs.anima.myEcharts();
            //         }else {
            //             this.$message("fail")
            //         }
            //     })
            // },
            handSelect(){
                // eslint-disable-next-line no-unused-vars
                var cid=this.$refs.adminMenu.cid
                this.$axios.get('/graph/'+cid).then(resp=>{
                    if(resp.data.code===200){
                        this.$refs.anima.edges=[]
                        this.$refs.anima.nodes=[]
                        this.preprocess(resp.data.result['nodes'],resp.data.result['edges'])
                        this.$refs.anima.myEcharts();
                    }else {
                        this.$message(resp.data.message)
                    }
                })
            },
            searchResult(){
                if(this.$refs.searchBar.queryMode==='0'){
                    this.$axios.get("/search?keywords="+this.$refs.searchBar.keywords).then(resp=>{
                        if (resp.data.code===200){
                            this.$refs.searchBar.keywords=[]
                            this.$refs.anima.edges=[]
                            this.$refs.anima.nodes=[]
                            this.preprocess(resp.data.result['nodes'],resp.data.result['edges'])
                            // console.log(this.$refs.anima.nodes)
                            this.$refs.anima.myEcharts();
                        }else {
                            this.$message(resp.data.message)
                        }
                    })
                }else{
                    this.$axios.get('/message/'+this.$refs.searchBar.keywords).then(resp=>{
                        if(resp.data.code===200){
                            // console.log(resp.data)
                            this.disease=this.$refs.searchBar.keywords
                            this.$refs.searchBar.keywords=[]
                            this.$refs.anima.edges=[]
                            this.$refs.anima.nodes=[]
                            this.preprocess(resp.data.result['nodes'],resp.data.result['edges'])
                            this.$refs.anima.myEcharts();
                        }else {
                            this.$message("fail")
                        }
                    })
                }
            },
            preprocess(nodes,edges){
                if (nodes.length===1){
                    this.$alert('此图谱尚未开放,敬请期待')
                }

                for (let i=0;i<nodes.length;i++){
                    let node=nodes[i]
                    if( node['type']===1){
                        this.disease=node['name']
                        this.$refs.anima.nodes.push(
                            {
                                id: node['id'],
                                draggable: true,  //可拖拽
                                name: node['name'],  //节点名称
                                symbolSize: 30,
                                label: {
                                    show: true
                                },
                                itemStyle:{
                                    color: '#ec0469',
                                }
                            }
                        )
                    }
                    else if( node['type']===2){
                        this.$refs.anima.nodes.push(
                            {
                                id: node['id'],
                                draggable: true,  //可拖拽
                                name: node['name'],  //节点名称
                                symbolSize: 20,  //节点大小，必须指明，否则节点大小为0，不显示图谱,
                                label: {
                                    show: true,
                                },
                                itemStyle:{
                                    color: '#66ccff',
                                }
                            }
                        )
                    }
                    else if( node['type']===3){
                        this.$refs.anima.nodes.push(
                            {
                                id: node['id'],
                                draggable: true,  //可拖拽
                                name: node['name'],  //节点名称
                                symbolSize: 10,  //节点大小，必须指明，否则节点大小为0，不显示图谱,
                                itemStyle:{
                                    color: '#ffbf04',
                                },
                                label:{
                                    fontSize:15
                                }
                            }
                        )
                    }
                    else if( node['type']===4){
                        this.disease=node['name']
                        this.$refs.anima.nodes.push(
                            {
                                id: node['id'],
                                draggable: true,  //可拖拽
                                name: node['name']+" (查询结点)",  //节点名称
                                symbolSize: 30,
                                label: {
                                    show: true
                                },
                                itemStyle:{
                                    color: '#31ee03',
                                }
                            }
                        )
                    }
                    else if( node['type']===5){

                        this.$refs.anima.nodes.push(
                            {
                                id: node['id'],
                                draggable: true,  //可拖拽
                                name: node['name']+" (查询结点)",  //节点名称
                                symbolSize: 20,  //节点大小，必须指明，否则节点大小为0，不显示图谱,
                                label: {
                                    show: true
                                },
                                itemStyle:{
                                    color: '#31ee03',
                                }
                            }
                        )
                    }
                    else if( node['type']===6){
                        this.disease=node['name']
                        this.$refs.anima.nodes.push(
                            {
                                id: node['id'],
                                draggable: true,  //可拖拽
                                name: node['name']+" (查询结点)",  //节点名称
                                symbolSize: 30,  //节点大小，必须指明，否则节点大小为0，不显示图谱,
                                label: {
                                    show: true
                                },
                                itemStyle:{
                                    color: '#31ee03',
                                }
                            }
                        )
                    }
                    else if( node['type']===7){
                        this.$refs.anima.nodes.push(
                            {
                                id: node['id'],
                                draggable: true,  //可拖拽
                                name: node['name'],  //节点名称
                                symbolSize: 20,  //节点大小，必须指明，否则节点大小为0，不显示图谱,
                                label: {
                                    show: true
                                },
                                itemStyle:{
                                    color: '#ec0469',
                                }
                            }
                        )
                    }
                    else if( node['type']===8){
                        this.$refs.anima.nodes.push(
                            {
                                id: node['id'],
                                draggable: true,  //可拖拽
                                name: node['name'],  //节点名称
                                symbolSize: 20,  //节点大小，必须指明，否则节点大小为0，不显示图谱,
                                label: {
                                    show: true
                                },
                                itemStyle:{
                                    color: '#ffbf04',
                                }
                            }
                        )
                    }
                }

                for (let i=0;i<edges.length;i++){
                    let edge=edges[i]
                    if( edge['type']===1){
                        this.$refs.anima.edges.push(
                            {
                                id: edge['id'],
                                source: edge['source'],
                                target: edge['target']
                            }
                        )
                    }
                    else if( edge['type']===2){
                        this.$refs.anima.edges.push(
                            {
                                id: edge['id'],
                                source: edge['source'],
                                target: edge['target'],
                                label: {
                                    show: true,
                                    formatter:'不良反应',
                                },
                            }
                        )
                    }
                    else if( edge['type']===3){
                        this.$refs.anima.edges.push(
                            {
                                id: edge['id'],
                                source: edge['source'],
                                target: edge['target'],
                                label: {
                                    show: true,
                                    formatter:'使用禁忌'
                                },
                            }
                        )
                    }
                    else if( edge['type']===4){
                        this.$refs.anima.edges.push(
                            {
                                id: edge['id'],
                                source: edge['source'],
                                target: edge['target'],
                                label: {
                                    show: true,
                                    formatter:'注意事项'
                                },
                            }
                        )
                    }
                    else if( edge['type']===5){
                        this.$refs.anima.edges.push(
                            {
                                id: edge['id'],
                                source: edge['source'],
                                target: edge['target'],
                                label: {
                                    show: true,
                                    formatter:'用法用量'
                                },
                            }
                        )
                    }
                    else if( edge['type']===6){
                        this.$refs.anima.edges.push(
                            {
                                id: edge['id'],
                                source: edge['source'],
                                target: edge['target'],
                                label: {
                                    show: true,
                                    formatter:'适应症状'
                                },
                            }
                        )
                    }
                }
                
                
                
            }
        }
    }
</script>

<style scoped>
    .login-container {
        border-radius: 15px;
        background-clip: padding-box;
        margin: 30px 330px;
        width: 1400px;
        padding: -35px -135px -35px 35px;
        background: #fff;
        border: 1px solid #eaeaea;
        box-shadow: 0 0 25px #cac6c6;
        opacity: 0.8;
    }
</style>