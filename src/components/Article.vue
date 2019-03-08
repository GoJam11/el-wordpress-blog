<template>
  <el-timeline class="articles" v-loading="loading">
    <div v-if="render">
      <Post v-for="post in rawdata" :post="post" :key="post.id"></Post>
    </div>
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
      page: 1,
      categories: "",
      render: false
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
            this.loadCategories(res.data);
            for (let item in res.data) {
              this.rawdata.push(res.data[item]);
            }
            this.page++;
            this.loading = false;
          })
          .catch(error => {
            console.log(error);
            this.loading = false;
            window.removeEventListener("scroll", this.winScroll);
          });
      }
    },
    loadCategories(data) {
      if (this.categories == "") {
        axios
          .get("https://www.guohere.com/wp-json/wp/v2/categories", {
            params: { per_page: 100 }
          })
          .then(res => {
            console.log("this");
            this.categories = res.data;
            for (let post in data) {
              //遍历文章
              for (let i in data[post].categories) {
                //遍历文章所属类别

                for (let category in this.categories) {
                  //遍历categories
                  if (
                    data[post].categories[i] == this.categories[category].id
                  ) {
                    data[post].categories[i] = this.categories[category].name;
                  }
                  //console.log(this.categories[category].id);
                  //console.log(this.categories[category].name);
                  //console.log("3:" + data[post].categories[i]);
                }
                //console.log(data[post].categories[i]);
              }
              //console.log("next");
            }
            this.render = true;
          });
      } else {
        for (let post in data) {
          //遍历文章
          for (let i in data[post].categories) {
            //遍历文章所属类别

            for (let category in this.categories) {
              //遍历categories
              if (data[post].categories[i] == this.categories[category].id) {
                data[post].categories[i] = this.categories[category].name;
              }
              //console.log(this.categories[category].id);
              //console.log(this.categories[category].name);
              //console.log("3:" + data[post].categories[i]);
            }
            //console.log(data[post].categories[i]);
          }
          //console.log("next");
        }
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
        this.loadCategories(this.rawdata);
      });
    window.addEventListener("scroll", this.winScroll);
  }
};
</script>
<style scoped>
.articles {
  padding-inline-start: 0px;
  -webkit-padding-start: 0px;
  width: 75%;
}
@media (max-width: 750px) {
  .articles {
    width: 100%;
  }
}
</style>