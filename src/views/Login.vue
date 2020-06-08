<template>
  <div class="login">
      <h1>用户登录</h1>
      <form>
        <div class="login-field">
          <input type="text" v-model="username">
          <label>用户名</label>
         </div>
        <div class="login-field">
          <input type="password" v-model="pwd">
          <label>密码</label>
        </div>
		<div class="log-btn">
          <b-button type="submit" @click="login">登录</b-button>
          <router-link to="/register"><b-button id="link-reg">注册</b-button></router-link>	
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
			pwd:'',
		}
	},
	methods:{
		login(){
			let that = this
			this.$av.User.logIn(this.username, this.pwd).then(() => {
				alert("登录成功，点击确定跳转首页")
				setTimeout(function(){
				that.$router.push({path:"/"})
				that.$router.go(0)
			}, 2000);
			}, (error) => {
				alert("登录失败，账号或密码错误")
				console.log(error)
			});
		}
	}
}
</script>

<style scoped>
	.login{
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
	
	.login h1 {
		margin: 20px 0 50px 0;
		padding: 0;
		text-align: center;
		color: #fff;
	}
	
	.login .login-field {
		position: relative;
		
	}
	
	.login .login-field input {
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
	
	.login .login-field label {
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
	
	.login .login-field input:focus ~ label,
	.login .login-field input:valid ~ label {
		top: -24px;
		left: 0;
		color: orange;
		font-size: 16px;
	}
	
	.login button{
		border-color: #000;
		background-color:orange;
		color: #fff;
		width: 250px;
		padding: 10px;
		cursor: pointer;
		transition: 0s;
    }
  
	.login button:hover {
		background-color: orange;
		border-color: #000;
		padding: 10px;
		cursor: pointer;
		color: #000;
		font-weight: bold;
    }

	#link-reg{
		border-color: orange;
		background-color: rgba(255,255,255,.1);
		color: orange;
    }
  
	#link-reg:hover {
		color: #000;
	}
	
	.log-btn{
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 20px;
	}
</style>