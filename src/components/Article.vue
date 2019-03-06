<template>
  <el-timeline style="padding-inline-start:0px;" class="article" v-loading="loading">
    <Post v-for="post in rawdata" :post="post" :key="post.id"></Post>
  </el-timeline>
</template>
<script>
import Post from './Post'
import axios from "axios";
export default {
  name: "BlogArticle",
  components:{
      Post
  },
  data:function(){
      return {
          rawdata:{},
          loading:true
      }
  },
  created:function(){
      axios.get("https://guohere.com/wp-json/wp/v2/posts").then(res => {
      console.log(res.data);
      this.rawdata = res.data;
      this.loading= false;
    });
  }
};
</script>
<style>

</style>