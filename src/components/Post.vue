<template>
  <el-timeline-item :timestamp="parsedtime" placement="top" class="box-card">
    <el-card style="margin-top:10px">
      <div slot="header" class="clearfix">
        <b><span v-html="post.title.rendered" class="title" @click="collapse"></span></b>
        <el-button style="float: right; padding: 3px 0" type="text">
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
}
.title:hover{
  color: #409eff
}
.new {
  color: #409eff;
  text-decoration: none;
}
</style>
