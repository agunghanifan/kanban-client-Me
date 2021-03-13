<template>
  <div>
    <div class="container-login">
      <div class="square-login">
        <div class="card-login">
          <img src="../img/logo_transparent.png" alt="kanban" width="400px" />
        </div>
        <div class="card-login">
          <span class="header-login-form">
            <h1>Login Here</h1>
          </span>
          <div class="alert alert-danger" role="alert" v-if="alertShow">
            {{loginError}}
          </div>
          <form @submit.prevent="login">
            <div class="mb-3">
              <label for="email-login" class="form-label">Email address</label>
              <input
                v-model="email"
                type="email"
                class="form-control"
                id="email-login"
                aria-describedby="emailHelp"
              />
              <div id="emailHelp" class="form-text">
                We'll never share your email with anyone else.
              </div>
            </div>
            <div class="mb-3">
              <label for="password-login" class="form-label">Password</label>
              <input 
              v-model="password" 
              type="password" 
              class="form-control" 
              id="password-login" />
            </div>
            <input
              @page="switchPage('kanban-board')"
              type="submit"
              class="btn btn-primary"
              value="Submit"
            />
          </form>
          <div class="mt-1">
            <GoogleLogin class="btn btn-primary" :params="params" :onSuccess="onSuccess" :onFailure="onFailure">Login With Google</GoogleLogin>
          </div>
          <div class="register-button mt-2">
            <input
              @click.prevent="switchPage('register-page')"
              type="submit"
              class="btn btn-primary"
              value="Register"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import GoogleLogin from 'vue-google-login';

export default {
  name: "loginPage",
  data() {
    return {
      email: "",
      password: "",
      params: {
        client_id: "1090907051704-s1vj79reb861vgcooomcfi0r425vmfh7.apps.googleusercontent.com"
      },
      renderParams: {
        width: 250,
        height: 50,
        longtitle: true
      },
      alertShow: false,
    }
  },
  methods: {
    login() {
      this.$emit("emitLogin", {email: this.email, password: this.password})
      this.showAlert()
       
    },

    showAlert() {
      console.log("masuk show alert")
      console.log(this.loginError)
      let setData = setInterval(() => {
        if(this.loginError) {
          console.log(this.loginError)
          this.alertShow = true
          this.email = ""
          this.password = ""
        } else {
          this.alertShow = false
          this.email = ""
          this.password = ""
        }
        clearInterval(setData);
      }, 2000);
    },

    switchPage() {
      this.$emit("emitSwitchRegisterPage", "register-page")
    },

    onSuccess(googleUser) {
      // console.log(googleUser);
      // This only gets the user information: id, name, imageUrl and email
      // console.log(googleUser.getBasicProfile());
      let data = googleUser.getAuthResponse().id_token
      // console.log(data)
      this.googleSignIn(data)
    },

    onFailure(error) {
      console.log(error)
    }

  },
  components: {
    GoogleLogin
  },
  props: [
    "googleSignIn",
    "loginError"
  ]
};
</script>

<style>
</style>