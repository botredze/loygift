<template>
<div class="authorization">
  <div class="container-fluid">
  <div class="sign-in-header">
    <img class="main-logo" src="../../assets/img/sign-logo.svg">
    <select class="select-lang form-control">
      <option>Eng</option>
      <option>Rus</option>
    </select>
  </div>

  <div class="login">
   <h1 class="welcome-sign-in">Welcome<br> to loy <span>Gift</span></h1>
    <div  v-if=" addErrorClass === true" class="text-left d-flex justify-content-between mb-3"><span>Incorrect username or password</span><img @click="closeErrorClass" src="../../assets/icons/redX.svg"/></div>
    <form @submit.prevent="loginSubmit">
      <label class="label">Login</label>
      <input v-model="login.email" name="email" class="login-input">
      <label class="label">Password</label>
      <div class="password d-flex justify-space-between align-items-center "><input  name="password" v-model="login.password" id="password"  class="login-input" style="background: transparent" type="password"><img id="hide-eye" @click="showPassword" src="../../assets/icons/Hide.svg"><img id="show-eye"  @click="showPassword" src="../../assets/icons/eye.svg"></div>

        <div class="remind d-flex justify-content-between align-item-center">
          <div class="d-flex ">
            <label class="custom-checkbox"><input type="checkbox"><span class="checkmark"></span></label>
            <span>Stay signed in</span>
          </div>
          <a href="/">Forgot your password?</a>
        </div>
      <button class="sign-in-btn" type="submit">Sign In</button>
    </form>

    <div class="main-or d-flex align-items-center justify-content-center">
      <div class="or-div"></div>
      <span class="or-text">or</span>
      <div class="or-div"></div>
    </div>

    <div class="have-account">Don't have an account? <a  @click="$router.push('/signup')">Sign up now</a></div>


  </div>

  <p class="footer-text">Cashback. Loyalty program. Client base. Offline cashback.
    Growing your business with one Mobile Application.</p>
  </div>
</div>
</template>

<script>
import $ from 'jquery';
export default {
name: "SignIn",
  data(){
  return{
    login:{
      email:'',
      password:'',

    },
    errorClass:false
  }
  },
  computed:{
    addErrorClass(){
      return this.errorClass;
    }
  },


  methods:{
    closeErrorClass(){
      this.errorClass = false;
    },
    showPassword: function () {
      var x = document.getElementById("password");
      if (x.type === "password") {
        x.type = "text";
        $('#hide-eye').css({'display':'none'})
        $('#show-eye').css({'display':'block'})
      } else {
        x.type = "password";
        $('#show-eye').css({'display':'none'})
        $('#hide-eye').css({'display':'block'})

      }
    },
    loginSubmit(){

      const data  = new FormData();
      data.append('email', this.login.email)
      data.append('password', this.login.password)
      this.axios.post(this.url('login'), data)
      .then((resp)=>{
        localStorage.setItem('token', resp.data.token)
        localStorage.setItem('user',JSON.stringify(resp.data.object))
        this.changeToken();
        this.$router.push('/orders')
      })
      .catch(()=>{
        localStorage.removeItem('token')
        this.errorClass= true;
        this.login.password = ''

      })
    }
  }
}
</script>

<style scoped>
.login .text-left{
  color:#E94A4A;
  background: #FFECEC;
  padding: 10px 15px;
  border-radius:5px;
}
.login{
  width: 586px;
  background: #FFFFFF;
  border: 1px solid #D3D3D3;
  box-sizing: border-box;
  border-radius: 10px;
  margin: 0 auto;
  padding:35px 90px;
  text-align: center;
  margin-bottom: 50px;

}

form{
  text-align: left;
}
.label{
  color:#222;
  margin-bottom: 8px;
  margin-right: 10px;
}


.remind{
  margin-bottom: 60px;
  color:#858585;
}
.remind a, .have-account a{
  color: #616CF5;
  text-decoration: none;
}
.remind label{
  margin-right: 10px;
}
.or-div{
  height: 0;
  width: 130px;
  border-bottom:1px solid #b0b0b0;
}
.or-text{
  margin:0 20px;
  color:#b0b0b0;
  font-size:18px;

}
.main-or{
  margin-top:15px;
  margin-bottom: 11px;
}
.have-account{

  color:#b0b0b0;
}

.welcome-sign-in{
  font-size:48px;
  font-weight: bold;
  color: #616CF5;
  line-height: 100%;
  margin-bottom: 38px;
}
.welcome-sign-in span{
  font-weight: 100;
}
.select-lang{
  width: 73px;
  height: 30px;
  font-size: 13px;

  background-position-x:80%;
  background-position-y: 12px;
}

.sign-in-header{
  display: flex;
  align-items: center;justify-content: space-between;
}

.have-account a{
  cursor:pointer;
}
.have-account a:hover{
  color:#616cf5
}
@media(max-width:768px){
  .login{
    width: 100%;
    padding: 0;
    border:none;
  }
  .authorization{
    background: #fff;
    padding: 0;
  }
  .sign-in-header{
    justify-content: center;
  }
  .select-lang{
    display:none;
  }
}

@media(max-width:576px){
  .authorization{
    padding:23px 0;
  }
  .welcome-sign-in{
    font-size: 36px !important;
  }
}
</style>