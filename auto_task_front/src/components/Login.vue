<template>
  <div>
    <section>
      <h2>Login</h2>
      <form>
        <input type="email" v-model="user.email" placeholder="Email" />
        <input type="password" v-model="user.password" placeholder="password" />
        <button v-on:click="onSubmit">Log in</button>
        <button>
          <router-link v-bind:to="'/Register'">Sign up</router-link>
        </button>
      </form>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {},
      token: ""
    };
  },
  methods: {
    onSubmit(e) {
    e.preventDefault();
    // generate token for api
    console.log(`${this.user.email} ${this.user.password}`);
    this.$http.post("http://localhost:3001/user_token/", {
      auth: {
        email: this.user.email, 
        password: this.user.password
        }
      })
    .then(jwt => {
        // console.log(JSON.parse(JSON.stringify(jwt.data)).jwt);
        const data = JSON.parse(JSON.stringify(jwt.data))
        localStorage.setItem('idToken', data.jwt)
        const jwtHeader = {'Authorization': 'Bearer ' + localStorage.getItem('idToken')}
        // console.log(localStorage.getItem('idToken'));
      if ( typeof localStorage.getItem('idToken') !== 'undefined' || localStorage.getItem('idToken') !== null) {
        this.$router.push({ path: "/tasks" });
        return this.$http.get('http://localhost:3001/tasks/', {headers: jwtHeader})
      };

      })
      .catch(err => {
        console.log(err)
        this.$router.push({ path: "/" })
        alert("Not Authorized");
      }); 
    
    }
  },
  created(){
    localStorage.removeItem('idToken');
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
html,
body {
  /* background-color: #95EEFF; */
  width: 100%;
  height: 100%;
  margin: 0px;
  font-family: "Work Sans", sans-serif;
}

body {

  background-size: cover;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #fff;
}

section {
  background-color: rgba(24, 25, 29, 0.72);
  color: aliceblue;
  border: solid 1px #ff1d00;
  width: 35%;
  min-height: 35%;
  display: flex;
  flex-direction: column;
  margin-left: auto;
  margin-right: auto;
}
form {
  display: flex;
  flex-direction: column;
  padding: 50px;
}
h2 {
  padding: 10px;
  font-size:4rem; 
  font-family: "Archivo Black", sans-serif;
  color: #e0dada;
  margin-left: auto;
  margin-right: auto;
}
input {
  height: 35px;
  padding: 5px 5px;
  margin: 10px 0px;
  background-color: #e0dada;
  border: none;
}
button, a {
  height: 40px;
  padding: 5px 5px;
  margin: 10px 0px;
  font-weight: bold;
  background-color: #be5256;
  border: none;
  color: #4A4343;
  cursor: pointer;
  font-size: 16px; 
}
button:hover {
  background-color: #4A4343;
  border: solid 1px #be5256;
  color: #be5256;
}

a:hover {
  text-decoration: none;
  background-color: #4A4343;
  color: #be5256;
}

@-webkit-keyframes shake {
  from,
  to {
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
  }
  10%,
  30%,
  50%,
  70%,
  90% {
    -webkit-transform: translate3d(-10px, 0, 0);
    transform: translate3d(-10px, 0, 0);
  }
  20%,
  40%,
  60%,
  80% {
    -webkit-transform: translate3d(10px, 0, 0);
    transform: translate(10px, 0, 0);
  }
}

.shake {
  animation-name: shake;
  animation-duration: 1s;
  /*animation-fill-mode: both;*/
}
@media screen and (max-width: 780px) {
  section {
    width: 90%;
  }
}
</style>
