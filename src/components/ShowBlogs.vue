<template>
    <div v-theme:column="'wide'" id="show-blogs">
        <h1>博客总览</h1>
        <input type="text" v-model="search" placeholder="请输入搜索内容">
        <div v-for="blog in filteredBlogs" class="single-blog">
          <router-link v-bind:to="'/blog/'+blog.id">
           <h2 v-rainbow>{{blog.title | toUppercase}}</h2>
          </router-link>
           <article>{{blog.content | snippet}}</article>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'show-blogs',
  data () {
    return {
        blogs:[],
        search:""
    }
  },
  created(){
      // this.$http.get('https://my-blog-d048a.firebaseio.com/posts.json')
      axios.get('https://my-blog-d048a.firebaseio.com/posts.json')
      .then(function(data){
        // console.log(data.json());
        return data.data;
        // this.blogs =  data.body.slice(0,10);
        // console.log(this.blogs);
      })
      .then((data) => {
        var blogsArray = [];
        for(let key in data){
            // console.log(data[key]);
            data[key].id = key;
            blogsArray.push(data[key]);
            // console.log(blogsArray);
            this.blogs = blogsArray;
        }
      })
  
    },
    computed:{
      filteredBlogs:function(){
        return this.blogs.filter((blog) =>{
          return blog.title.match(this.search);
        })
      }
    },

    filters:{
      // "to-uppercase":function(){
      //   return value.toUpperCase();
      // }
      toUppercase(value){
        return value.toUpperCase();
      },
    },
     directives:{
        'rainbow':{
          bind(el,binding,vnode){
            el.style.color = "#" + Math.random().toString(16).slice(2,8);
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
#show-blogs h1{
  text-align: center;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
    border: 1px dotted #aaa;
}
#show-blogs a{
    color: #444;
    text-decoration: none;
}
input[type="text"]{
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
}
</style>