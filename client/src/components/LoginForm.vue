<template>
  <div class="container mt-5">
      <div class="wrapper fadeInDown" v-if="isLoginPage">
        <div id="formContent">
          <h2 class="active" @click.prevent="setIsLoginPage(true)"> Login </h2>
          <h2 class="inactive underlineHover" @click.prevent="setIsLoginPage(false)"> Register </h2>

          <div class="fadeIn first p-3">
            <!-- <img src="" id="icon" alt="User Icon" /> -->
          </div>

          <form class="m-3 p-3" @submit.prevent='login()'>
            <input type="email" class="fadeIn second" placeholder="Your Email Here" v-model="email">
            <input type="password" class="fadeIn third" placeholder="Password" v-model="password">
            <input type="submit" class="fadeIn fourth" value="Log In">
          </form>

        </div>
      </div>
      <div class="wrapper fadeInDown" v-else>
        <div id="formContent">
          <h2 class="inactive underlineHover" @click.prevent="setIsLoginPage(true)"> Login </h2>
          <h2 class="active" @click.prevent="setIsLoginPage(false)"> Register </h2>

          <div class="fadeIn first p-3">
            <!-- <img src="" id="icon" alt="User Icon" /> -->
          </div>

          <form class="m-3 p-3" @submit.prevent='register()'>
            <input type="text" class="fadeIn second" placeholder="Username" v-model="username">
            <input type="email" class="fadeIn second" placeholder="Your Email Here" v-model="email">
            <input type="password" class="fadeIn second" placeholder="Password" v-model="password">
            <input type="submit" class="fadeIn fourth" value="Register">
          </form>

        </div>
      </div>
  </div>
</template>

<script>
import Swal from 'sweetalert2'
export default {
    name : "LoginForm",
    data() {
        return {
            isLoginPage : true,
            email : '',
            username: '',
            password : ''
        }
    },
    methods : {
        setIsLoginPage( status ) {
            this.isLoginPage = status
        },
        login() {
            this.$store.dispatch('login' , {
                email : this.email,
                password : this.password
            })
            .then( response => {
                console.log( response.data.token )
                const token = response.data.token;
                const user = response.data.user;
                localStorage.setItem('token' , token );
                localStorage.setItem('user' , JSON.stringify( user ) );
                
                this.$store.commit('setIsLogin' , true )
                this.$store.commit('setUser' , user )
                this.$router.push('/questions')
                
                this.resetField()

                Swal.fire('Login Success' , '' , 'success')

            })
            .catch( err => {
              if( err.response.data )
                Swal.fire('Login Failed' , err.response.data , 'error')
              else console.log( err )
            })
        },
        register() {
            this.$store.dispatch('register' , {
                email : this.email,
                username : this.username,
                password : this.password
            })
            .then( res => {
                Swal.fire('Register Success' , 'Please login to continue' , 'success' );
                this.resetField()
            })
            .catch( err => {
                Swal.fire('Register Failed' , err.response.data[0] , 'error')
            })
        },
        resetField() {
            this.email = ''
            this.username = ''
            this.password = ''
        }
    }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Poppins');

html {
  background-color: #56baed;
}

body {
  font-family: "Poppins", sans-serif;
  height: 100vh;
}

a {
  color: #92badd;
  display:inline-block;
  text-decoration: none;
  font-weight: 400;
}

h2 {
  text-align: center;
  font-size: 16px;
  font-weight: 600;
  text-transform: uppercase;
  display:inline-block;
  margin: 40px 8px 10px 8px; 
  color: #cccccc;
}



.wrapper {
  display: flex;
  align-items: center;
  flex-direction: column; 
  justify-content: center;
  width: 100%;
  min-height: 100%;
  padding: 20px;
}

#formContent {
  -webkit-border-radius: 10px 10px 10px 10px;
  border-radius: 10px 10px 10px 10px;
  background: #fff;
  padding: 30px;
  width: 90%;
  max-width: 450px;
  position: relative;
  padding: 0px;
  -webkit-box-shadow: 0 30px 60px 0 rgba(0,0,0,0.3);
  box-shadow: 0 30px 60px 0 rgba(0,0,0,0.3);
  text-align: center;
}

#formFooter {
  background-color: #f6f6f6;
  border-top: 1px solid #dce8f1;
  padding: 25px;
  text-align: center;
  -webkit-border-radius: 0 0 10px 10px;
  border-radius: 0 0 10px 10px;
}



h2.inactive {
  color: #cccccc;
}

h2.active {
  color: #0d0d0d;
  border-bottom: 2px solid #5fbae9;
}



/* FORM TYPOGRAPHY*/
input[type=button], input[type=submit], input[type=reset]  {
  background-color: #56baed;
  border: none;
  color: white;
  padding: 15px 80px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  text-transform: uppercase;
  font-size: 13px;
  -webkit-box-shadow: 0 10px 30px 0 rgba(95,186,233,0.4);
  box-shadow: 0 10px 30px 0 rgba(95,186,233,0.4);
  -webkit-border-radius: 5px 5px 5px 5px;
  border-radius: 5px 5px 5px 5px;
  margin: 5px 20px 40px 20px;
  -webkit-transition: all 0.3s ease-in-out;
  -moz-transition: all 0.3s ease-in-out;
  -ms-transition: all 0.3s ease-in-out;
  -o-transition: all 0.3s ease-in-out;
  transition: all 0.3s ease-in-out;
}

input[type=button]:hover, input[type=submit]:hover, input[type=reset]:hover  {
  background-color: #39ace7;
}

input[type=button]:active, input[type=submit]:active, input[type=reset]:active  {
  -moz-transform: scale(0.95);
  -webkit-transform: scale(0.95);
  -o-transform: scale(0.95);
  -ms-transform: scale(0.95);
  transform: scale(0.95);
}

input[type=text],input[type=password] ,input[type=email] {
  background-color: #f6f6f6;
  border: none;
  color: #0d0d0d;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 5px;
  width: 85%;
  border: 2px solid #f6f6f6;
  -webkit-transition: all 0.5s ease-in-out;
  -moz-transition: all 0.5s ease-in-out;
  -ms-transition: all 0.5s ease-in-out;
  -o-transition: all 0.5s ease-in-out;
  transition: all 0.5s ease-in-out;
  -webkit-border-radius: 5px 5px 5px 5px;
  border-radius: 5px 5px 5px 5px;
}

input[type=text]:focus , input[type=password]:focus ,input[type=email]:focus {
  background-color: #fff;
  border-bottom: 2px solid #5fbae9;
}

input[type=text]::placeholder , input[type=password]::placeholder ,input[type=email]::placeholder{
  color: #cccccc;
}



/* ANIMATIONS */

/* Simple CSS3 Fade-in-down Animation */
.fadeInDown {
  -webkit-animation-name: fadeInDown;
  animation-name: fadeInDown;
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
}

/* Simple CSS3 Fade-in Animation */
@-webkit-keyframes fadeIn { from { opacity:0; } to { opacity:1; } }
@-moz-keyframes fadeIn { from { opacity:0; } to { opacity:1; } }
@keyframes fadeIn { from { opacity:0; } to { opacity:1; } }

.fadeIn {
  opacity:0;
  -webkit-animation:fadeIn ease-in 1;
  -moz-animation:fadeIn ease-in 1;
  animation:fadeIn ease-in 1;

  -webkit-animation-fill-mode:forwards;
  -moz-animation-fill-mode:forwards;
  animation-fill-mode:forwards;

  -webkit-animation-duration:1s;
  -moz-animation-duration:1s;
  animation-duration:1s;
}

.fadeIn.first {
  -webkit-animation-delay: 0.2s;
  -moz-animation-delay: 0.2s;
  animation-delay: 0.2s;
}

.fadeIn.second {
  -webkit-animation-delay: 0.3s;
  -moz-animation-delay: 0.3s;
  animation-delay: 0.3s;
}

.fadeIn.third {
  -webkit-animation-delay: 0.4s;
  -moz-animation-delay: 0.4s;
  animation-delay: 0.4s;
}

.fadeIn.fourth {
  -webkit-animation-delay: 0.5s;
  -moz-animation-delay: 0.5s;
  animation-delay: 0.5s;
}

/* Simple CSS3 Fade-in Animation */
.underlineHover:after {
  display: block;
  left: 0;
  bottom: -10px;
  width: 0;
  height: 2px;
  background-color: #56baed;
  content: "";
  transition: width 0.2s;
}

.underlineHover:hover {
  color: #0d0d0d;
}

.underlineHover:hover:after{
  width: 100%;
}



/* OTHERS */

*:focus {
    outline: none;
} 

#icon {
  width:60%;
}

* {
  box-sizing: border-box;
}
</style>