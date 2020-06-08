<!--  -->
<template>
     <div class="top flex">
      <div class="left">
        <img src="@/assets/logo.png" class="logo"/>
        <router-link to="/" class="tabbar-item">首页</router-link>
        <router-link to="/index" class="tabbar-item">游戏</router-link>
        <router-link to="/about" class="tabbar-item">资讯</router-link>
      </div>
      <div class="right flex">
        <input class="search" placeholder="请输入搜索" v-model="find" @keyup.enter="search">
        <span class="search-btn"><img src="@/assets/icon/search.png" class="search-icon" @click="search"></span>
        <router-link to="/login" class="login tabbar-item" v-if="flag==false" >登录</router-link>
        <router-link to="/login" class="reg tabbar-item" v-if="flag==false">注册</router-link>
        <b-dropdown :text="nickname" variant="dark" class="user" size="sm" v-if="flag">
           <b-dropdown-item href="#" @click="toNext">个人中心</b-dropdown-item>
           <b-dropdown-item href="#" @click="toNext">我的文章</b-dropdown-item>
           <b-dropdown-item href="#" @click="toNext">我的点赞</b-dropdown-item>
           <b-dropdown-item href="#" @click="logOut">登出</b-dropdown-item>
        </b-dropdown>
      </div>
    </div>

</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
    name:"navbar",
    //import引入的组件需要注入到对象中才能使用
    components: {},
    data() {
    //这里存放数据
      return {
      tabIndex:1,
      flag:false,
      nickname:'',
      find:'',
    };
  },
  methods:{
    toNext(){
      this.$router.push({
        path:"/my"
      })
    },
    logOut(){
      this.$av.User.logOut()
      this.$router.go(0)
    },
    search(){
      if(this.find){
      this.$router.push({
        path:"/search",
        query:{key:this.find}
      })
      this.find = ''
    }
  }
  },
  beforeMount(){
      const currentUser = this.$av.User.current()
      const userQuery = new this.$av.Query('_User')
      let id = currentUser.id
      if (currentUser) {
        this.flag = true
        userQuery.get(id).then((res)=>{
          console.log(res)
          this.nickname = res.attributes.nickname
        })
      } else {
      this.flag = false
      }
    }
}
</script>

<style>
.top{
  width: 100%;
  background: rgba(1, 1, 1, 0.9);
  z-index: 999;
  justify-content: space-between;
  padding:0 40px;
  box-sizing: border-box;
  box-shadow: 0 3px 10px rgba(1,1,1,0.9);
  position: fixed;
}
.tabbar-item{
  color: rgb(221, 221, 221);
  margin-right: 40px;
}
.tabbar-item:hover{
   color: burlywood;
  text-decoration:none;
}

.logo{
  height: 40px;
  width: 40px;
  margin: 0 20px;
}
.right{
  width:600px;
  justify-content: flex-end;
}
.flex{
  display: flex;
  align-items: center;
  flex-wrap:wrap;
}

.search{
  width: 300px;
  border-radius: 20px 0 0 20px;
  height: 25px;
  font-size: 13px;
  line-height: 25px;
  padding: 0 10px;
  outline:none;
  border:0;
}
.search-btn{
  height: 25px;
  border-radius: 0 20px 20px 0;
  width: 30px;
  background: #fff;
  margin-right: 80px;
}

.search-icon{
  width: 17px;
  height: 17px;
  cursor:pointer;/*鼠标指针变为一个手*/ 
}

.user{
  margin: 0 ;
}
a.router-link-exact-active{
  color: burlywood;
}

input:focus{ outline:none; }

</style>