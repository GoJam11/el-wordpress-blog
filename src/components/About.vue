<template>
    <el-card class="card"  v-loading="loading">
      <div slot="header" class="clearfix ">
    <span class="title pagetitle"><b>关于</b></span>
    <el-button style="float: right; padding: 3px 0" type="text"><a target="_blank" :href="url">新窗口</a></el-button>
  </div>
  <div v-html="rawdata" class="content"></div>
    </el-card>
</template>
<script>
import axios from "axios";
export default {
    data:function(){
        return{
            rawdata:'Loading...',
            url:'',
            loading:true
        }
    },created(){
        axios.get("https://www.guohere.com/wp-json/wp/v2/pages?include=8").then(res=>{
            this.rawdata=res.data[0].content.rendered
            this.url=res.data[0].link
            this.loading=false
        })
        
    }
    
}
</script>
<style>
.card{
    margin-top:30px;
    font-size: 14px;
    min-height: 200px;
    width: 100%
}
.pagetitle{
    font-size: 20px
}
title:hover{
    color: #409eff
}

.content img{
max-width: 250px;
}

</style>
