<template>
  <el-timeline-item :timestamp="parsedtime" placement="top" class="box-card">
    <el-card style="margin-top:10px">
      <div slot="header" class="clearfix">
        <b>
          <span v-html="post.title.rendered" class="title" @click="collapse"></span>
        </b>
        <el-tag
          size="small"
          class="title-tag"
          v-for="category in post.categories"
          :key="category"
        >{{category}}</el-tag>
        <el-button
          class="right-link"
          style="float: right; padding: 3px 0;position:relative;top:3px"
          type="text"
        >
          <a :href="post.link" target="_blank" class="new">新窗口</a>
        </el-button>
      </div>
      <div class="text item" v-html="content" @click="collapse"></div>
    </el-card>
  </el-timeline-item>
</template>

<script>
import moment from "moment";
export default {
  name: "post",
  props: {
    post: Object
  },
  computed: {
    parsedtime: function() {
      return moment(this.post.date).format("YYYY-MM-DD");
    },
    content() {
      return this.collapsed
        ? this.post.excerpt.rendered
        : this.post.content.rendered;
    }
  },
  created() {},
  data: function() {
    return {
      collapsed: true
    };
  },
  methods: {
    collapse(e) {
      this.collapsed = !this.collapsed;
    }
  }
};
</script>

<style >
.box-card {
  margin: 0px 0px 0px 0px;
}
.text {
  font-size: 14px;
  overflow-x: scroll;
}
.text::-webkit-scrollbar {
  display: none;
}

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}
.title {
  font-size: 20px;
  cursor: pointer;
}
.title:hover {
  color: #409eff;
}
.new {
  color: #409eff;
  text-decoration: none;
}

.title-tag {
  margin-left: 5px;
  margin-right: 5px;
  position: relative;
  bottom: 2px;
}
.card-header {
  line-height: 50px;
}
</style>
