<template>
  <div>
    <div class="article-li">
      <ul>
        <li><label :class="tab==0?'active':''" @click="changeTab(0)">全部</label></li>
        <li><label :class="tab==1?'active':''" @click="changeTab(1)">精品</label></li>
        <li><label :class="tab==2?'active':''" @click="changeTab(2)">攻略</label></li>
        <li><label :class="tab==3?'active':''" @click="changeTab(3)">杂谈</label></li>
      </ul>
    </div>

   <div class="main">
    <div class="information">
      <div class="info" v-for="(item,index) in showList" :key="index" @click="toCon(item.id)">
        <img class="info-img" :src="item.attributes.img.attributes.url">
        <div class="info-text">{{item.attributes.title}}</div>
      </div>
    </div>
  </div>

  </div>
</template>

<script>
export default {
  data(){
    return{
      tab:0,
      artList:[],
      showList:[]
    }
  },
  methods:{
      getArticle(){
        const query = new this.$av.Query('information');
        query.descending('createdAt');
        query.find().then(res => {
        console.log("看看文章",res)
        this.artList = res
        this.showList = res
        });
      },
      toCon(artId){
        let id = artId
         this.$router.push({ 
           path:'/about/content',
           query: {id:id}
           })
      },
    changeTab(num){
      this.tab = num
      const query = new this.$av.Query('information')
      switch(num){
        case 0:
          this.showList = this.artList
          break;
        case 1:
          query.equalTo("good",true)
          query.find().then(res=>{
            this.showList = res
          })
          break;
        case 2:
           query.equalTo("classify","攻略")
           query.find().then(res=>{
            this.showList = res
          })
          break;
        case 3:
          query.equalTo("classify","杂谈")
           query.find().then(res=>{
            this.showList = res
          })
          break;
      }
    }
  },
  beforeMount(){
    this.getArticle()
  }
}
</script>

<style scoped>

.article-li{
    margin: 20px auto;
    line-height: 30px;
    width: 80%;
    height: 30px;
    color:#fff;
}

ul{
    width: 100%;
    display: flex;
    justify-content: flex-start;
    margin-left: 20px;
}
li{
    list-style: none;
    width: 100px;
}

li label{
    font-size: 24px;
    cursor: pointer;
}

.information{
  margin-bottom: 40px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
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
.main{
  width: 1500px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 100px;
  flex-wrap: wrap;
}
.active{
  color: #D9A86C;
}
</style>
