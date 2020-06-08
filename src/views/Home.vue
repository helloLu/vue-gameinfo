<template>
  <div class="home">
    <Carousel msg="轮播图" class="lunbo" />
   
    <div class="games">
       <div class="hot">
         <h2>热门游戏</h2>
         <div class="more-btn" @click="toIndex">查看全部</div>
         </div>
      <div class="game" v-for="(item,index) in gameList" :key="index" @click="toGame(item.id)">
        <img :src="item.attributes.img.attributes.url" class="game-img">
        <h6 class="game-name">{{item.attributes.name}}</h6>
        <span class="classify" v-for="(item2,index2) in item.attributes.classify" :key="index2">{{item2}}</span>
      </div>
    </div>

     <div class="games">
       <div class="hot"><h2>新发布游戏</h2><div class="more-btn" @click="toIndex">查看全部</div></div>
      <div class="game" v-for="(item,index) in hotList" :key="index" @click="toGame(item.id)">
        <img :src="item.attributes.img.attributes.url" class="game-img">
        <h6 class="game-name">{{item.attributes.name}}</h6>
        <span class="classify" v-for="(item2,index2) in item.attributes.classify" :key="index2">{{item2}}</span>
      </div>
    </div>

    <div class="information">
       <div class="hot"><h2>最新资讯</h2><div class="more-btn" @click="toAbout">查看全部</div></div>
      <div class="info" v-for="(item,index) in artList" :key="index" @click="toCon(item.id)">
        <img class="info-img" :src="item.attributes.img.attributes.url">
        <div class="info-text">{{item.attributes.title}}</div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Carousel from '@/components/Carousel.vue'

export default {
    name: 'Home',
    data(){
      return{
        gameList:[],
        artList:[],
        hotList:[],
      }
    },
    components: {
        Carousel
    },
    methods:{
      getNew(){
        const query = new this.$av.Query('game');
        query.descending('createdAt');
        query.limit(5);
        query.find().then(res => {
        console.log("看看",res)
        this.gameList = res
        });
      },
       getHot(){
        const query = new this.$av.Query('game');
        query.equalTo('hot',true);
        query.limit(5);
        query.find().then(res => {
        console.log("看看",res)
        this.hotList = res
        });
      },
      getArticle(){
        const query = new this.$av.Query('information');
        query.descending('createdAt');
        query.limit(4);
        query.find().then(res => {
        console.log("看看文章",res)
        this.artList = res
        });
      },
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
      toIndex(){
        this.$router.push({
          path:"/index"
        })
      },
      toAbout(){
        this.$router.push({
          path:"/about"
        })
      }
    },
    created(){
      this.getNew()
      this.getHot()
      this.getArticle()
    }
}
</script>

<style scoped>
.home{
  margin: 0 auto;
 width:1500px;
 padding-bottom: 100px;
}
.hot{
  color: #fff;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.more-btn{
  padding: 5px 10px;
  border: 1px solid #fff;
  cursor:pointer;
}
.lunbo{
  width:1500px;
  margin: 0 auto;
}
.games{
  width: 100%;
  margin-top: 40px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap:wrap ;
}
.game{
  margin-top: 40px;
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
.information{
  margin-bottom: 40px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  margin-top: 40px;
}
.info{
  width: 730px;
  background: rgb(34, 34, 34);
  margin: 20px 0;
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