<template>
  <div id="add-blog" v-theme:column="'wide'">
      <h2>添加博客</h2>
      <form v-if="!subminted">
        <label>博客标题</label>
        <input type="text" v-model="blog.title" />
        
        <label>博客内容</label>
        <textarea v-model="blog.content"></textarea>

        <div id="checkboxes">
          <label>Vue.js</label>
          <input type="checkbox" value="Vue.js" v-model="blog.categories">
          <label>Node.js</label>
          <input type="checkbox" value="Node.js" v-model="blog.categories">
          <label>Rect.js</label>
          <input type="checkbox" value="Rect.js" v-model="blog.categories">
          <label>Angular.js</label>
          <input type="checkbox" value="Angular.js" v-model="blog.categories">
        </div>
        <label>作者：</label>
        <select v-model="blog.author">
          <option v-for="author in authors">
            {{author}}
          </option>
        </select>
        <button v-on:click.prevent="post">添加博客</button>
      </form>
  
      <div v-if="subminted">
        <h3>你的博客发送成功了！</h3>
      </div>

      <div id="preview">
        <h3>博客总览</h3>
        <p>博客标题:{{blog.title}}</p>
        <p>博客内容:</p>
        <p>{{blog.content}}</p>
        <p>博客分类</p>
        <ul>
          <li v-for="category in blog.categories">
            {{category}}
          </li>
        </ul>
        <p>作者：{{blog.author}}</p>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'add-blog',
  data () {
    return {
      blog:{
        title:"",
        content:"",
        categories:[],
        author:""
      },
      authors:["TaylorSwift","Jerry","Tom"],
      subminted:false
    }
  },
  methods:{
    post:function(){
      // var _this = this;
      axios.post('https://my-blog-d048a.firebaseio.com/posts.json',this.blog)
        .then((data) => {
          // console.log(data);
          this.subminted = true;
        });
    }
  }
}
</script>
#

<style scoped>

#add-blog *{
 box-sizing: border-box;
}

#add-blog{
  margin: 20px auto;
  max-width: 600px;
  padding: 20px;
}

#add-blog h2{
  text-align: center;
}

label{
  display: block;
  font-weight:bold;
  margin: 20px 0 10px;
}

input[type="text"],textarea,select{
  display: block;
  width: 100%;
  padding: 8px;
}


textarea{
  height: 200px;
}

#checkboxes label{
  display:inline-block;
  margin-top: 0;
}

#checkboxes input{
  display: inline-block;
  margin-right: 10px;
}

button{
  display: block;
  margin: 20px 0;
  background:crimson;
  color: #ffffff;
  border: 0;
  padding: 14px;
  border-radius: 6px;
  font-size: 18px;
  cursor: pointer;
}

#preview{
  padding: 10px 20px;
  border: 3px dotte #ccc;
  margin:30px 0;
}

h3{
  margin-top: 10px;
}
</style>
