<template>
  <div v-theme:column="'width'" id="show-blogs">
    <h1>博客总览</h1>
    <input type="text" placeholder="搜索" v-model="search">
    <div class="single-blog" v-for="(blog,index) in filteredBlogs" v-bind:key="index">
      <router-link v-bind:to="'/blog/'+blog.id">
        <h2 v-rainbow>{{blog.title|to-uppercase}}</h2>
      </router-link>
      <article>
        {{blog.body|snippet}}
      </article>
    </div>
  </div>
</template>

<script>

export default {
  name: 'show-blog',
  data(){
    return{
      blogs:[],
      search:""
    }
  },
  created(){
    // this.$http.get("https://jsonplaceholder.typicode.com/posts")
    //   .then(function(data){
    //     this.blogs=data.body.slice(0,10);
    //     console.log(this.blogs);
    //   })

    this.$http.get("http://localhost:8080/posts.json")
      .then(function(data){
        this.blogs=data.body.slice(0,10);
        console.log(this.blogs);
      })
  },
  computed:{
    filteredBlogs:function(){
      return this.blogs.filter((blog) =>{
        return blog.title.match(this.search);
      })
    }
  },
  // 本地过滤器
  filters:{
    "to-uppercase":function(value){
      return value.toUpperCase();
    }
    // ES6写法
    // toUppercase(value){
    //   return value.toUpperCase();
    // }
  },
  // 本地指令
  directives:{
    "rainbow":{
      bind(el,binding,vnode){
        el.style.color="#"+Math.random().toString(16).slice(2,8);
      }
    }
  }
}
</script>

<style scoped>
#show-blogs{
  max-width: 800px;
  margin: 0 auto;
}
.single-blog{
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background-color: #eee;
  border: 1px dotted #aaa;
}
#show-blogs a{
  color: #444;
  text-decoration: none;
}
input[type="text"]{
  padding: 8px;
  width:100%;
  box-sizing: border-box;
}
</style>
