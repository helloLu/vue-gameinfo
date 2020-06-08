<template>
  <div class="main">
      <h3 class="count">共找到{{gameList.length+artList.length}}个结果：</h3>
    <h4>游戏：</h4>
    <div class="gameList">
      <div class="game" v-for="(item,index) in gameList" :key="index" @click="toGame(item.id)">
        <img :src="item.attributes.img.attributes.url" class="game-img">
        <h6 class="game-name">{{item.attributes.name}}</h6>
        <span class="classify" v-for="(item2,index2) in item.attributes.classify" :key="index2">{{item2}}</span>
      </div>
    </div>
    <h4>文章：</h4>
    <div class="artList">
       <div class="info" v-for="(item,index) in artList" :key="index" @click="toCon(item.id)">
        <img class="info-img" :src="item.attributes.img.attributes.url">
        <div class="info-text">{{item.attributes.title}}</div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
    data(){
        return{
            gameList:[],
            artList:[],
        }
    },
    methods:{
        toCon(artId){
        let id = artId
         this.$router.push({ 
           path:'/about/content',
           query: {id:id}
           })
      },
       toGame(gameId){
        let id = gameId
        this.$router.push({
          path:"/game",
          query:{id:id}
        })
      },
    },
    beforeMount(){
        let key = this.$route.query.key
        const query = new this.$av.Query("game")
        query.contains("name",key)
        query.find().then(res=>{
            this.gameList = res
            console.log(res)
        })
        const query2 =new this.$av.Query("information")
        query2.contains("title",key)
        query2.find().then(res=>{
            this.artList = res
             console.log(res)
        })
    }
}
</script>

<style scoped>

.main{
    padding: 40px;
}
h4{
    color: #fff;
}
.count{
    color: #fff;
    margin-bottom: 40px;
}
.gameList{
    display:flex;
    justify-content: flex-start;
    align-items: flex-start;
    flex-wrap: wrap;
}
.game{
  margin-bottom: 40px;
  margin-right: 25px;
  width: 280px;
  height: 425px;
  color: #fff;
  background: rgb(34, 34, 34);
}
.game:hover{
  box-shadow: 5px 5px 10px 5px rgba(0,0,0,0.6);
  cursor:pointer;/*鼠标指针变为一个手*/ 
}
.game-img{
  width: 100%;
  height: 370px;
  object-fit: cover;
}
.game-name{
  text-align: left;
  margin: 5px 0 0 0;
}
.classify{
  width: 60px;
  background: gray;
  height: 20px;
  margin-right: 5px;
  font-size:12px;
  padding: 2px 4px;
}
.artList{
    display:flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
}
.info{
  width: 730px;
  background: rgb(34, 34, 34);
  margin-bottom:20px;
}
.info-img{
  width: 100%;
  height: 400px;
  background: slategrey;
  object-fit: cover;
}
.info-text{
  padding: 10px 20px; 
  box-sizing: border-box;
  font-size: 20px;
  color: #fff;
}
.info:hover{
  box-shadow: 5px 5px 10px 5px rgba(0,0,0,0.6);
  cursor:pointer;/*鼠标指针变为一个手*/ 
}
</style>