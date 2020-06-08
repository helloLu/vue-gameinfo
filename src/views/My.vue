<template>
  <div class="my">
      <div class="main">
          <h5>我的创作</h5>
          <div class="two">
            <div class="arts addNew" @click="toSetArt">
                <b-icon icon="plus-circle-fill" variant="secondary"  font-scale="7.5"></b-icon>
            </div>
            <div class="arts" v-for="(item,index) in list" :key="index">
                <img :src="item.attributes.img.attributes.url" alt="" class="art-img">
                <div class="title">{{item.attributes.title}}</div>
            </div>
          </div>
          <h5>我的点赞</h5>
          <div class="two">
            <div class="arts" v-for="(item,index) in likeList" :key="index">
                <img :src="item.img.attributes.url" alt="" class="art-img">
                <div class="title">{{item.title}}</div>
            </div>
            </div>
      </div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            list:[],
            likeList:[],
        }
    },
    methods:{
        toSetArt(){
            this.$router.push({
                path:"/setarticle"
            })
        }
    },
    beforeMount(){
        let nickname = this.$av.User.current().attributes.nickname
        let id = this.$av.User.current().id
        let query =new this.$av.Query("information")
        query.equalTo("author",nickname)
        query.find().then(res=>{
           this.list = res
        })
        console.log(nickname,id)
        let query2 = new this.$av.Query("_User")
        query2.get(id).then((res)=>{
            
            let list = res.attributes.like
            console.log(list)
            for(let i = 0;i<list.length;i++){
                query.get(list[i]).then((todo)=>{
                    this.likeList.push(todo.attributes)
                })
            }
            console.log(this.likeList)
        })
    },

}
</script>

<style scoped>
.my  {
    height: 100%;
    margin: 0;
    width: 100%;
}
.main{
    color: #fff;
    padding:0 100px 40px 100px; 
}
h5{
    margin: 40px 0;
}
.two{
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    width: 100%;
    margin: 0 auto;
}
.arts{
    width: 500px;
    height: 300px;
    background: rgb(34, 34, 34);
    margin-bottom: 20px;
    margin-right: 40px;
}
.arts:hover{
  box-shadow: 5px 5px 10px 5px rgba(0,0,0,0.6);
  cursor:pointer;/*鼠标指针变为一个手*/ 
}
.addNew{
    display: flex;
    align-items: center;
    justify-content: center;
}
.art-img{
    width: 500px;
    height: 250px;
    object-fit: cover;
}
.title{ 
  padding: 10px 20px; 
  box-sizing: border-box;
  font-size: 20px;
  color: #fff;
}
</style>