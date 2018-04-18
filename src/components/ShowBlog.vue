<template>
  <div id="show-blog" >
    <h1>博客总览</h1>
    <input type="text" placeholder="搜索" v-model="search">
    <div class="single-blog" v-for="blog in filteredBlogs">
      <router-link v-bind:to="'/blog/'+blog.id">
      <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
      </router-link>
      <article>
        {{blog.content  }}
      </article>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'show-blog',
    data(){
      return{
          blogs:[ ],
           search:"",
           stringa:""
      }
    },
    created(){
      this.$http.get("https://wd1182543348jfzvtq.wilddogio.com/posts.json").then(function(data){
        console.log(data.json());
        return data.json()
      }).then(function(data){
        var blogsArray=[ ];
        for(let key in data){
          console.log(key);
          console.log(data[key]);
          data[key].id=key;
          blogsArray.push(data[key]);
        }
        this.blogs=blogsArray;
      })
    },
    computed:{
      filteredBlogs(){
        //调用了ES6的filter方法,返回产找的所有blogs,然后让搜索输入的内容与获取到的title进行匹配,没有输入返回所有
        return this.blogs.filter((blog)=>{
          return blog.title.match(this.search);
        })
      },

    },
    filters:{
      "to-uppercase":function(value){
        return value.toUpperCase();
      },
    },
    directives:{
      'rainbow':{
        bind(el, binding, vnode) {
          el.style.color = "#" + Math.random().toString(16).slice(2, 8);
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #show-blog{
    max-width: 800px;
    margin: 0 auto;
  }
  .single-blog{
   padding: 20px;
    margin:20px 0;
    box-sizing: border-box;
    background: #eee;
    border: 1px dotted #aaa;
  }
#show-blog a{
  text-decoration: none;
}
  input[type="text"]{
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
  }

</style>
