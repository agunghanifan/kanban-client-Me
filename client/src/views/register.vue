<template>
<div>
    <div class="container-login">
      <div class="square-login">
        <div class="card-login">
          <img src="../img/logo_transparent.png" alt="kanban" width="400px" />
        </div>
        <div class="card-login">
          <span class="header-login-form">
            <h1>Register Here</h1>
          </span>
          <div v-for="(error, index) in registerError" :key="index">
              <div class="alert alert-danger" role="alert" v-if="alertShow">
              {{error}}
              </div>
          </div>
          <form @submit.prevent="register">
            <div class="mb-3">
              <label for="name-register" class="form-label">Name</label>
              <input
                v-model="name"
                type="name"
                class="form-control"
                id="name-register"
              />
            </div>
            <div class="mb-3">
              <label for="email-register" class="form-label"
                >Email address</label
              >
              <input
                v-model="email"
                type="email"
                class="form-control"
                id="email-register"
                aria-describedby="emailHelp"
              />
              <div id="emailHelp" class="form-text">
                We'll never share your email with anyone else.
              </div>
            </div>
            <div class="mb-3">
              <label for="password-register" class="form-label">Password</label>
              <input
                v-model="password"
                type="password"
                class="form-control"
                id="password-register"
              />
            </div>
            <input
              type="submit"
              class="btn btn-primary"
              value="Submit"
            />
          </form>
          <div class="login-button mt-2">
            <input
              v-on:click.prevent="switchPage('login-page')"
              type="submit"
              class="btn btn-primary"
              value="Do you have an account? Click here!"
            />
          </div>
        </div>
      </div>
    </div>
</div>
</template>

<script>
export default {
  name: "registerPage",
  data() {
    return {
      name: "",
      email: "",
      password: "",
      alertShow: false
    }
  },

  methods: {
    switchPage(toPage) {
      this.$emit("page", toPage)
    },

    register() {
      this.$emit("emitRegister", {name: this.name, email: this.email, password: this.password})
      this.showAlert()
    },

    showAlert() {
      // console.log("masuk show alert")
      // console.log(this.registerError)
      let setData = setInterval(() => {
        if(this.registerError) {
          // console.log(this.registerError)
          this.alertShow = true
          this.name = ""
          this.email = ""
          this.password = ""
        } else {
          this.alertShow = false
          this.name = ""
          this.email = ""
          this.password = ""
        }
        clearInterval(setData);
      }, 2000);
    },
    
  },

  props: [
    "registerError",
  ]
};
</script>

<style>
</style>