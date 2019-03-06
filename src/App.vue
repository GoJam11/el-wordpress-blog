<template>
  <div id="app">
    <h1 class="site">果这里</h1>
    <Menu :activeIndex="activeIndex" v-on:handleSelect="handleSelect"></Menu>
    <div class="outer">
      <keep-alive>
        <component :is="currentCom"></component>
      </keep-alive>
    </div>
  </div>
</template>

<script>
import Menu from "./components/Menu";
import BlogArticle from "./components/Article";
import About from "./components/About";
import NotFound from "./components/404";
import axios from "axios";

export default {
  name: "app",
  components: {
    Menu,
    BlogArticle,
    About,
    NotFound
  },
  data: function() {
    return {
      ComList: {
        BlogArticle: "1",
        About: "4"
      },
      ComRoute: {
        "/BlogArticle": "BlogArticle",
        "/": "BlogArticle",
        "/About": "About"
      },
      path: window.location.pathname.match(/\/[^\/\\]*$/)[0]
    };
  },
  computed: {
    currentCom: function() {
      return this.ComRoute[this.path] || "NotFound";
    },
    activeIndex: function() {
      return this.ComList[this.currentCom] || "0";
    }
  },
  methods: {
    handleSelect(key, keyPath) {
      //console.log(key, keyPath);
      //this.activeIndex = key;
      //window.location.pathname=key
      this.path = window.location.pathname.match(/\/[^\/\\]*$/)[0];
    }
  },
  created() {
    console.log();
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #202122;
}

#app img{
  max-width: 100%
}

body {
  margin: 0;
}

.site {
  text-align: center;
  margin-block-start: 0em;
  margin-block-end: 0em;
  padding-block-start: 0.67em;
  padding-block-end: 0.67em;
  z-index: 2;
}

.outer {
  margin: 20px 40px 0px 40px;
}
@media (max-width: 750px) {
  .outer {
    margin: 20px 10px 0px 10px;

 
  }
}

a {
  color: #409eff;
  text-decoration: none;
}
</style>
