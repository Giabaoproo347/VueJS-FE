<template>
  <Categories/>
  <Posts/>
</template>

<script>


import Categories from "@/components/categories/Categories";
import Posts from "@/components/posts/PostList";

export default {
  name: 'App',
  components: {
    Categories,
    Posts
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
