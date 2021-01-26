<template>
    <div >
        <el-dropdown @command="handleCommand">
  <span class="el-dropdown-link" style="color: #eaeaea;margin-right: 20px" >
    选择模式<i class="el-icon-arrow-down el-icon--right"></i>
  </span>
            <el-dropdown-menu slot="dropdown">
                <el-dropdown-item command="0">普通查询</el-dropdown-item>
                <el-dropdown-item command="1">文献查询</el-dropdown-item>
            </el-dropdown-menu>
        </el-dropdown>
        <el-input v-if="queryMode==='0'" prefix-icon="el-icon-search" v-model="keywords" placeholder="普通查询  请输入疾病或药名"
                  @keyup.enter.native="onClick" style="width: 400px;margin-right: 30px;height: 40px"></el-input>
        <el-input v-if="queryMode==='1'" prefix-icon="el-icon-search" v-model="keywords" placeholder="文献查询 请输入药名"
                  @keyup.enter.native="onClick" style="width: 400px;margin-right: 30px;height: 40px"></el-input>
        <el-button icon="el-icon-search" @click="onClick" style="background-color: #42b983;color: white">搜索</el-button>
    </div>
</template>

<script>
    export default {
        name: "SearchBar",
        data(){
            return{
                queryMode:'0',
                keywords:''
            }
        },

        methods:{
            handleCommand(command){
              this.queryMode=command
            },


            onClick(){
                this.$emit("onSearch")
            }
        }
    }
</script>

<style scoped>

</style>