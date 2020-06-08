<template>
  <div class="games">
      <div class="header">
      <div class="classify">
          <h5 :class="flag=='all'?'active':''" @click="orderBy('all')" class="c-item">全部</h5>
          <h5 :class="flag=='hot'?'active':''" @click="orderBy('hot')" class="c-item">热门</h5>
          <h5 :class="flag=='new'?'active':''" @click="orderBy('new')" class="c-item">最新</h5>
          <h6 v-for="(item,index) in classList" :key="index" 
          @click="changeTag(item,index)" :class="flag==index?'active':''"
          class="c-item">{{item}}</h6>  
      </div>
      </div>

    <div class="main">
       <div class="game" v-for="(item,index) in gameList" :key="index" @click="toGame(item.id)">
        <img :src="item.attributes.img.attributes.url" class="game-img">
        <h6 class="game-name">{{item.attributes.name}}</h6>
        <span class="tags" v-for="(item2,index2) in item.attributes.classify" :key="index2">{{item2}}</span>
      </div>
    </div>

  </div>
</template>

<script>
export default {
    data(){
        return{
            flag:"all",
            classList:["第一人称","开放世界","模拟","动作","解密","角色扮演"],
            gameList:[],
            gameAll:[],
        }
    },
    methods:{
      getAll(){
        this.gameList = this.gameAll
      },
      orderBy(str){
        console.log(str)
        switch(str){
          case "all":
            this.flag = "all"
            this.gameList = this.gameAll
            break;
          case "hot":
            this.flag = "hot"
            this.getHot()
            break;
          case "new":
            this.flag = "new"
            this.getNew()
            break;
        }
      },
        getNew(){
        const query = new this.$av.Query('game');
        query.descending('createdAt');
        query.find().then(res => {
        console.log("看看",res)
        this.gameList = res
        });
      },
      toGame(gameId){
        let id = gameId
        this.$router.push({
          path:"/game",
          query:{id:id}
        })
      },
      changeTag(str,index){
        console.log(str)
        this.flag = index
        console.log(this.flag)
        const query = new this.$av.Query('game');
        query.contains("classify",str)
        query.find().then(res=>{
          this.gameList = res
        })
      },
      getHot(){
        const query = new this.$av.Query('game');
        query.equalTo('hot',true);
        query.find().then(res => {
        console.log("看看",res)
        this.gameList = res
        });
      },
    },
    created(){
        const query = new this.$av.Query('game');
        query.find().then(res => {
        console.log("看看",res)
        this.gameList = res
        this.gameAll = res
        });
    },
}
</script>

<style scoped>
.header{
    width: 1500px;
     margin: 0 auto;
}
.main{
    display: flex;
    align-items: center;
    width: 1525px;
    margin: 0 auto;
    flex-wrap: wrap;
    padding-bottom: 100px;
}
.main:after{
    content: "";display: block; height:0; width:280px;
      /*只需要添加父元素的after伪元素中 高度0 ，宽度与item的宽一样*/
}
.classify{
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    width: 550px;
    color: #FFF;
    margin-top:20px;
}
.c-item:hover{
  color: burlywood;
  cursor:pointer;/*鼠标指针变为一个手*/ 
}
.game{
  margin-top: 20px;
  width: 280px;
  height: 425px;
  color: #fff;
  background: rgb(34, 34, 34);
  margin-right: 25px;
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
.tags{
  width: 60px;
  background: gray;
  height: 20px;
  margin-right: 5px;
  font-size:12px;
  padding: 2px 4px;
}
.active{
    color: burlywood;
}
</style>