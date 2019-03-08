<template>
  <el-menu
    :default-active="activeIndex"
    class="el-menu-demo menu"
    mode="horizontal"
    @select="handleSelect"
    active-text-color="#409eff"
  >
    <transition appear-class="enter" appear>
      <h1 class="nav_title">果这里</h1>
    </transition>
    <el-menu-item index="1">首页</el-menu-item>
    <el-submenu index="2" disabled>
      <template slot="title">目录</template>
      <el-menu-item index="2-1">搬代码</el-menu-item>
      <el-menu-item index="2-2">折腾</el-menu-item>
      <el-menu-item index="2-3">日常</el-menu-item>
      <el-submenu index="2-4">
        <template slot="title">更多</template>
        <el-menu-item index="2-4-1">选项1</el-menu-item>
        <el-menu-item index="2-4-2">选项2</el-menu-item>
        <el-menu-item index="2-4-3">选项3</el-menu-item>
      </el-submenu>
    </el-submenu>
    <el-menu-item index="3" disabled>我的收藏夹</el-menu-item>
    <el-menu-item index="4">关于</el-menu-item>
  </el-menu>
</template>

<script>
export default {
  name: "blog-menu",
  props: {
    activeIndex: String,
    items: Object
  },
  data: function() {
    return {
      comRoute: {
        "1": "/BlogArticle",
        "4": "/About"
      }
    };
  },
  methods: {
    handleSelect(key, keyPath) {
      history.pushState(
        {},
        "",
        window.location.pathname.match(/(.*)\/[^\/\\]*$/)[1] +
          this.comRoute[key]
      );
      this.$emit("handleSelect", key, keyPath);
    },

    enter: function(el, done) {}
  }
};
</script>
<style scoped>
.menu {
  width: 100%;
  display: flex;
  justify-content: space-between;
  box-shadow: 0 5px 6px 0 rgba(0, 0, 0, 0.05);
  position: sticky;
  top: 0;
  z-index: 1;
  font-weight: 900;
}

@media (max-width: 750px) {
  .nav_title {
    display: none;
  }
}

@media (min-width: 750px) {
  .nav_title {
    margin: 0;
    line-height: 60px;
    display: block;
    transition: all 0.3s;
    font-size: 32px;
  }
  .enter {
    transform: translate(-280px, 0px);
  }
  .nav_title:hover::first-letter {
    color: #409eff;
  }
  .nav_title:hover {
    animation: shake 0.5s;
  }
  @keyframes shake {
    33%{
transform: rotate(20deg);
    }
    66%{
      transform: rotate(-20deg);
    }
    100% {
      
    }
  }
  /**.nav_title:hover::after{
    background-color: #409eff;
    height: 100%;
    width: 100%;
    display: inline-flex;
    content: ''
  }**/
}
</style>
