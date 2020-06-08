<template>
  <div class="main">
      <div class="game">
          <img :src="img" alt="" class="header">
          <h3 class="name">游戏名称：尼尔机械纪元</h3>
          <div class="info">
              <div class="info-detail">
                <span class="info-name">厂商：</span>
                <span>{{company}}</span>
              </div>
              <div class="info-detail">
                <span class="info-name">发行日期：</span>
                <span>{{date}}</span>
              </div>
             <div class="info-detail">
                <span class="info-name">平台：</span>
                <span>{{platform}}</span>
              </div>
              <div class="info-detail">
                <span class="info-name">标签：</span>
                <span>{{tags}}</span>
              </div>
          </div>
          <div class="info-detail detail">
              <span class="info-name"> 简介：</span>
              <span>{{detail}}</span>
            </div>
      </div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            company:"",
            date:"",
            platform:"",
            tags:"",
            detail:"",
            img:"",
        }
    },
    methods:{

    },
    beforeMount(){
        if(this.$route.query.id){
        let id = this.$route.query.id
        console.log("看看1",id)
        let query = new this.$av.Query("game")
        query.get(id).then(res=>{
            console.log("查询结果",res)
            this.company = res.attributes.company
            this.date = res.attributes.date
            this.platform = new String(...[res.attributes.platform])
            this.tags = new String(...[res.attributes.classify])
            this.detail = res.attributes.detail
            this.img = res.attributes.img.attributes.url
        })}
        if(this.$route.query.game){
             let game = this.$route.query.game
             console.log("看看2",game)
             let query2 = new this.$av.Query("game")
             query2.equalTo("name",game)
             query2.find().then(fuck=>{
            console.log("看看Fuck",fuck)
            let res = fuck[0]
            this.company = res.attributes.company
            this.date = res.attributes.date
            this.platform = new String(...[res.attributes.platform])
            this.tags = new String(...[res.attributes.classify])
            this.detail = res.attributes.detail
            this.img = res.attributes.img.attributes.url
             })
        }
    }
}
</script>

<style scoped>
.main{
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
}
.game{
    width: 1200px;
    background: rgb(34,34,34);
     box-shadow: 5px 5px 10px 5px rgba(0,0,0,0.6);
}
.header{
    width: 100%;
    height: 400px;
    object-fit: cover;
}
.name{
    color: #fff;
    text-align: center;
    padding-top: 20px;
}
.info{
    color: #fff;
    font-size: 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px 40px;
}
.info-detail{
    display: flex;
    flex-direction: column;
}
.info-name{
    color: burlywood;
}
.detail{
    padding: 0 40px 40px 40px; 
    font-size: 20px;
    color: #fff;
}
</style>