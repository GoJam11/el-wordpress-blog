<template>
  <el-timeline class="articles" v-loading="loading">
    <Post v-for="post in rawdata" :post="post" :key="post.id"></Post>
  </el-timeline>
</template>
<script>
import Post from "./Post";
import axios from "axios";
export default {
  name: "BlogArticle",
  components: {
    Post
  },
  data: function() {
    return {
      rawdata: {},
      loading: true,
      page: 1
    };
  },
  methods: {
    winScroll: function(e) {
      if (window.scrollY + window.innerHeight == document.body.offsetHeight) {
        this.loading = true;
        axios
          .get("https://guohere.com/wp-json/wp/v2/posts", {
            params: { page: this.page }
          })
          .then(res => {
            for (let item in res.data) {
              this.rawdata.push(res.data[item]);
            }
            this.page++;
            this.loading = false;
          })
          .catch(error => {
            this.loading = false;
            window.removeEventListener("scroll", this.winScroll);
          });
      }
    }
  },
  created: function() {
    axios
      .get("https://guohere.com/wp-json/wp/v2/posts", { params: { page: 1 } })
      .then(res => {
        this.rawdata = res.data;
        this.page++;
        this.loading = false;
      });
    window.addEventListener("scroll", this.winScroll);
  }
};
</script>
<style scoped>
.articles {
  padding-inline-start: 0px;
  -webkit-padding-start: 0px;
}
</style>