<template>
  <div class="reg" id="reg">
      <h1>新用户注册</h1>
      <form>
        <div class="reg-field">
          <input type="text" v-model="username">
          <label>用户名</label>
        </div>
        <div class="reg-field">
          <input type="text" v-model="nickname">
          <label>昵称</label >
        </div>
        <div class="reg-field">
          <input type="password" v-model="pwd">
          <label>密码</label>
        </div>
        <div class="reg-button">
          <b-button type="submit" @click="reg">注册</b-button>
          <router-link to="/login">已有账号，登录 ></router-link>  
        </div>
      </form>
  </div>
</template>

<script>
export default {
    name: 'Login',
	data(){
		return{
			username:'',
			nickname:"",
			pwd:"",
		}
	},
	methods:{
		reg(){
			const user = new this.$av.User()
			user.setUsername(this.username)
			user.set('nickname', this.nickname);
			user.setPassword(this.pwd)
			let that = this
			user.signUp().then(() => {
			alert("注册成功，点击确定跳转登录")
			setTimeout(function(){
				that.$router.push({path:"/Login"})
				}, 2000);
			})
		}
	}
}
</script>

<style scoped>
	.reg{
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%,-50%);
		width: 600px;
		padding: 40px;
		background: rgba(0,0,0,.8);
		box-sizing: border-box;
		box-shadow: 0 15px 25px rgba(0,0,0,.5);
		border-radius: 10px;
	}
	
	.reg h1 {
		margin: 20px 0 50px 0;
		padding: 0;
		text-align: center;
		color: #fff;
	}
	
	.reg .reg-field {
		position: relative;
		
	}
	
	.reg .reg-field input {
		width: 100%;
		padding: 10px 0;
		font-size: 16px;
		color: #fff;
		margin-bottom: 50px;
		border: none;
		border-bottom: 1px solid #fff;
		outline: none;
		background: transparent;
	}
	
	.reg .reg-field label {
		position: absolute;
		top: 0;
		left: 0;
		letter-spacing: 1px;
		padding:  0;
		font-size: 16px;
		color: #fff;
		pointer-events: none;
		transition: .5s;		
	}
	
	.reg .reg-field input:focus ~ label,
	.reg .reg-field input:valid ~ label {
		top: -24px;
		left: 0;
		color: orange;
		font-size: 16px;
	}
	
	.reg button{
		border-color: #000;
		background-color:orange;
		color: #fff;
		width: 250px;
        padding: 10px;
		cursor: pointer;
		transition: 0s;
    }
  
	.reg button:hover {
		background-color: orange;
		border-color: #000;
		padding: 10px;
		cursor: pointer;
		color: #000;
		font-weight: bold;
    }

    #reg a{
        color: #fff;
        font-size: 14px;
    }

    #reg a:hover{
        color: orange;
    }

    .reg-button{
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-bottom: 10px;
    }
</style>