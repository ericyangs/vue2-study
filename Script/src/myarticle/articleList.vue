<template>
  <div class="article-list-wrap">
     <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>文章列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-row class="list-content">
       <el-col :span="5" class="content-left">
         <div class="btnwrap">
            <el-button type="primary" @click="showDialog">发布文章</el-button>
         </div>
         <div class="filterwrap">
           <span>筛选</span>
           <div>
           <el-date-picker
            v-model="daterange"
            size="small"
            type="daterange"
            placeholder="选择日期范围">
           </el-date-picker>
           </div>
           <div>
             <el-input
                placeholder="请输入关键字"
                icon="search"
                size="small"
                v-model="keysearch"
                :on-icon-click="keysearchFun">
             </el-input>
           </div>
           <el-row>
             <el-col :span="5">状态:</el-col>
             <el-col :span="19">
                <el-select v-model="stausvalue" clearable placeholder="请选择" size="small">
                  <el-option
                    v-for="item in statusoptions"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>
             </el-col>
           </el-row>
           <el-row>
             <el-col :span="5">标签:</el-col>
             <el-col :span="19"><el-input v-model="labelvalue" placeholder="请输入内容" size="small"></el-input></el-col>
           </el-row>
           <el-row>
             <el-col :span="5">类型:</el-col>
             <el-col :span="19">
                <el-select v-model="typevalue" clearable placeholder="请选择" size="small">
                  <el-option
                    v-for="item in typeoptions"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>
             </el-col>
           </el-row>
         </div>
       </el-col>
       <el-col :span="19" class="content-right">
          <div class="right-inner">
             <div class="title">我的文章</div>
                  <el-table
                    :data="tableData"
                    stripe
                    style="width: 100%">
                    <el-table-column
                    label="发布日期"
                    prop="ReleaseDateTime"
                    width="180">
                     <template slot-scope="scope">
                       {{scope}}
                     </template>
                    </el-table-column>
                    <el-table-column
                    prop="Title"
                    label="文章标题"
                    width="180">
                    </el-table-column>
                    <el-table-column
                    prop="State"
                    label="状态">
                    </el-table-column>

                    <el-table-column
                    prop="comment"
                    label="人气/点赞/评论">
                    </el-table-column>
                    <el-table-column
                    prop="CustomTypeName"
                    label="类型">
                    </el-table-column>
                    <el-table-column
                    prop="VId"
                    label="操作">
                    </el-table-column>
                </el-table>
          </div>
       </el-col>
    </el-row>
    <el-dialog 
      title="发布文章" 
      :visible.sync="visible"
      width="75%"
    >
    <dialog-content ref="dialogcontent"></dialog-content>
    <div slot="footer" class="dialog-footer">
        <el-button @click="visible = false">取 消</el-button>
        <el-button type="primary" @click="commit">确 定</el-button>
    </div>
  </el-dialog>
  </div>
</template>

<script>
  //引入样式文件
  import './style/index.less';
  //引入模拟数据
  import {tabledata} from './data.js';
  //引入弹窗组件
  import dialogContent from './dialogContent.vue';
  import request from '.././request.js';  
  export default {
      data(){
          return{
              daterange:'',
              keysearch:'',
              stausvalue:'',
              statusoptions:[],
              labelvalue:'',
              typevalue:'',
              typeoptions:[],
              coldata:[],
              tableData:tabledata,
              visible:false//控制弹窗显示与隐藏
          }
      },
      mounted:function(){
         this.loadTableData()
      },
      components:{
        "dialog-content":dialogContent
      },
      methods:{
          keysearchFun:function(){
          },
          showDialog:function(){
              this.visible = true
          },
          loadTableData:async function(){
            try{
              let data1 ={
                  // endDate:"2017-10-25",
                  pageIndex:1,
                  pageSize:10,
                  // startDate:"1970-01-01",
                  customArticleTypeID:1,
                  // userID:''
              }
              let options = {
                    url:'/api/services/Article/Article/GetArticleListByPage',
                    // url:'/api/services/Article/Article/GetArticleByID',
                    // url:'/api/services/payroll/setofbook/GetAllList',
                    // url:'/api/services/Article/Collection/GetCollectionListByPage',
                method:'post',
                data:data1
                } 
              let {data} =await request.post(options.url,data1);
              let tableData = JSON.parse(data.value).Array;
              this.tableData = tableData
            }catch(err){
                console.log(err) 
            }
          },
          commit:function(){
            this.visible = false;
            this.$refs.dialogcontent.submit(this.loadTableData)
          }
      } 
  } 
</script>

<style lang="less">

</style>