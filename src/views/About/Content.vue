<template>
<div class="container">
  <img :src="banner" alt="" class="banner">
  
  <div class="main">
    <div class="connect-game" v-if="gameName" @click="toGame">相关游戏：{{gameName}}</div>
    <div class="title">{{art.title}}</div>
    <div class="author">
      <span>作者：{{art.author}}</span>
      <span>{{time}}</span>
      <span>{{like}}喜欢</span>
      <span>分类：{{art.classify}}</span>
    </div>
    <div v-html="art.article">{{art.article}}</div>
       <b-button :pressed="flag" pill variant="outline-danger" class="mb-2 like-btn" @click="likeIt">
      <b-icon icon="heart" aria-hidden="true" class="heart-icon"></b-icon> {{like}}人喜欢
    </b-button>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      art:{},
      like:0,
      time:"",
      banner:'',
      flag:false,
      likeState:true,//false时不再增加点赞数量了
      id:'',
      userId:"",
      gameName:"",
    }
  },
  methods:{
    likeIt(){
        this.flag = !this.flag
        if(this.flag){
          this.like =  this.like+1
        }
        if(this.flag==false){
          this.like = this.like-1
      }
    },
    toGame(){
      this.$router.push({
        path:"/game",
        query:{game:this.gameName}
      })
    }
  },
  mounted(){
    console.log(this.$route.query.id)
    let id = this.$route.query.id
    this.id = id
    let query = new this.$av.Query('information')
    query.get(id).then(res=>{
      console.log("我是结果：",res)
      if(res.attributes.gamename)
      {this.gameName=res.attributes.gamename}
      this.art = res.attributes
      let date = res.createdAt
      let y = date.getFullYear()
      let m = date.getMonth() + 1//月份从0开始
      m = m < 10 ? '0' + m : m
      let d = date.getDate()
      d = d < 10 ? ('0' + d) : d
      this.time = y + '-' + m + '-' +d
      this.banner = res.attributes.img.attributes.url
      this.like = res.attributes.like//文章的点赞数
    })
    let query2 = new this.$av.Query("_User")
    let user = this.$av.User.current()
    let userid =user.id
    this.userId = userid
    let artId = this.id
    query2.get(userid).then(
      todo=>{
        console.log("这里",todo)
        let likeList = todo.attributes.like
        let res = likeList.indexOf(artId)
        if (res!==-1){
          this.flag = true
          this.likeState = false
          console.log("点过赞了吗",this.likeState)
        }
      })
  },
  beforeDestroy(){
    let query = new this.$av.Query("information")
    let query2 = new this.$av.Query("_User")
    let userId = this.userId
    //在数据库选中当前的文章
    query.get(this.id).then((todo)=>{
        if(this.flag&&this.likeState){
        todo.increment("like",1)//点赞数加一 
        todo.save().then(()=>{
          query2.get(userId).then((res)=>{
            res.add("like",this.id)
            console.log("加入","this.id")
            res.save()
          })
        })
      }
      if(!this.flag&&!this.likeState){
        todo.increment("like",-1)
         todo.save().then(()=>{
          query2.get(userId).then((res)=>{
            res.remove("like",this.id)
            console.log("去掉",this.id)
            res.save()
          })
        })
      }
    }) 
  }
}
</script>

<style scoped> 
.container{
  display: flex;
  flex-direction:column;
  align-items: center;
  justify-content: flex-start;
}
.main{
  padding: 40px;
  background: #fff;
  width: 800px;
}
.banner{
  width: 800px;
  height: 300px;
  object-fit: cover;
}
.connect-game{
  color:#fff;
  background:cornflowerblue;
}
.title{
  font-size: 36px;
}
.author{
  width: 350px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-left: 20px;
  margin-bottom: 30px;
  color: gray;
}
.like-btn{
  margin: 0 auto;
  display: flex;
  align-items: center;
  line-height: 1.5;
}
.heart-icon{
  margin-right: 10px;
}
</style>