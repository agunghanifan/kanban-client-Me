<template>
  <div>
    <!-- DONEEEEE -->
    <!-- LOGIN PAGE -->
    <loginPage :loginError="loginError" :googleSignIn="googleSignIn" @emitSwitchRegisterPage="switchPage" @emitLogin="login" v-if="page === 'login-page'"  id="login-page">
    </loginPage>
    <!-- DONEEEE  -->
    <!-- REGISTER PAGE -->
    <registerPage :registerError="registerError" @page="switchPage" @emitRegister="register"  v-else-if="page === 'register-page'" id="register-page">
    </registerPage>
  
    <!-- KANBAN BOARD -->
    <kanbanBoard :addError="addError" :deleteError="deleteError" :editError="editError" :moveData="moveData"  :deleteData="deleteData" :submitEditData="submitEditData" :showDataEdit="editDataShow" :fetchEditData="fetchEditData" :addData="addData" :greetings="message" :categoriesFromParent="categories" @page="switchPage" @logOut="logOut" :loadTaskFromParent="fetchData" :tasksFromParent="tasks" v-else-if="page === 'kanban-board'" id="kanban-page">
      
    </kanbanBoard>

    <!-- PAGE NOT FOUND -->
    <notFoundPage @page="switchPage" @logOut="logOut" v-else>
      
    </notFoundPage>
  </div>
</template>

<script>
import axios from "axios"
import loginPage from "./views/login.vue"
import registerPage from "./views/register.vue"
import kanbanBoard from "./views/kanban-board.vue"
import notFoundPage from "./views/not-found.vue"
const baseUrl = "https://kanban-board-agung-server.herokuapp.com"
// const baseUrl = "http://localhost:3000"

export default {
  name: "KanbanApp",

  data() {
    return {
      message: "",
      user: "",
      tasks: {},
      categories: [],
      page: 'login-page',
      editDataShow: "",
      loginError: "",
      registerError: [],
      editError: [],
      deleteError: [],
      addError: []
    }
  },

  created() {
    this.isLogin()
  },

  methods: {
    moveData(data, id) {
      let CategoryId
      let idTarget = id
      // console.log(data, idTarget)
      this.categories.forEach((category) => {
        if(category.nameCategory == data) {
          CategoryId = category.id
        }
      })
      // console.log(CategoryId)
      axios({
        url: baseUrl + `/kanban/task/${idTarget}`,
        method: "patch",
        headers: {
          access_token: localStorage.getItem("access_token")
        },
        data: {
          CategoryId 
        }
      })
        .then((response) => {
          // console.log("success move")
          this.fetchData()
        })
        .catch((error) => {
          // console.log(error)
          this.deleteError = error.response.data.message
          // console.log(this.deleteError)
        })

    },

    googleSignIn(data) {
      // console.log(id_token)
      axios({
        url: baseUrl + "/loginGoogle",
        method: 'POST',
        data: {
          id_token: data
        }
      })
        .then((response) => {
          // console.log(response)
          if(response) {
            localStorage.setItem("access_token", response.data.access_token)
            localStorage.setItem("name", response.data.name)
            this.isLogin()
          }
        })
        .catch((error) => {
          // console.log(error)
        })
    },

    deleteData(id) {
      axios({
        url: baseUrl + `/kanban/task/${id}`,
        method: "delete",
        headers: {
          access_token: localStorage.getItem("access_token")
        }
      })
        .then(() => {
          // console.log("success delete")
          this.fetchData()
        })
        .catch((error) => {
          // console.log(error)
          this.deleteError = error.response.data.message
          // console.log(this.deleteError)
        })
    },

    addData(data) {
      // console.log(data)
      const { title, description, assign_to, CategoryId, point } = data
      axios({
        method: "post",
        url: baseUrl + "/kanban/add",
        data: {
          title,
          description, 
          assign_to, 
          point,
          CategoryId
        },
        headers: {
          access_token: localStorage.getItem("access_token")
        }
      })
        .then((response) => {
          // console.log(response)
          this.fetchData()
        })
        .catch((error) => {
          // console.log(error)
          this.addError = error.response.data.message
          // console.log(this.addError)
        })
    },

    switchPage(toPage) {
      this.page = toPage
    },

    login(data) {
      let email = data.email
      let password = data.password
      
      axios({
          method: "post",
          url: baseUrl + "/login", 
          data: {
            email: email,
            password: password
          },
      })
        .then((response) => {
          if(response.data) {
            localStorage.setItem("access_token", response.data.access_token)
            localStorage.setItem("name", response.data.name)
            this.isLogin()
          }
          
        })
        .catch((error) => {
          this.loginError = error.response.data.message
          // console.log(this.loginError)
        })
    },

    isLogin() {
      if(localStorage.getItem("access_token")) {
        this.user = localStorage.getItem("name")
        this.message = `Hello ${this.user} Welcome to Kanban Boards!`
        this.page = "kanban-board"
        this.fetchData()
      } else {
        this.page = "login-page"
      }
    },

    logOut() {
      localStorage.removeItem("access_token")
      localStorage.removeItem("name")
      this.isLogin()
    },

    register(data) {
      // console.log(data)
      let name = data.name
      let email = data.email
      let password = data.password

      axios({
        url: baseUrl + "/register",
        method: "post",
        data: {
          name,
          email,
          password
        }
      })
        .then((response) => {
          this.isLogin()
        })
        .catch((error) => {
          // console.log(error)
          this.registerError = error.response.data.message
          // console.log(this.registerError)
        })
    },

    fetchData() {
      this.tasks = ""
      this.categories = ""
      axios({
        url: baseUrl + "/kanban",
        method: "get",
        headers: {
          access_token: localStorage.getItem("access_token")
        }
      })
        .then((response) => {
          this.tasks = response.data
          return axios({
            url: baseUrl + "/kanban/cat",
            method: "get",
            headers: {
              access_token: localStorage.getItem("access_token")
            }
          })
        })
        .then((response) => {
          this.categories = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    },

    fetchEditData(id) {
      // console.log(id)
      axios({
        url: baseUrl + `/kanban/task/${id}`,
        method: "get",
        headers: {
          access_token: localStorage.getItem("access_token")
        }
      })
        .then((response) => {
          this.editDataShow = response.data

        })
        .catch((error) => {
          console.log(error)
        })
    },

    submitEditData(data) {
      const { title, id, description, point, assign_to } = data
      axios({
        url: baseUrl + `/kanban/task/${id}`,
        method: "put",
        headers: {
          access_token: localStorage.getItem("access_token")
        },
        data: {
          title, 
          description, 
          assign_to,
          point
        }
      })
        .then((response) => {
          // console.log("success edit")
          this.fetchData()

        })
        .catch((error) => {
          // console.log(error)
          this.editError = error.response.data.message
          // console.log(this.editError)
        })
    }
    

  },
  components: {
    loginPage,
    registerPage,
    kanbanBoard,
    notFoundPage
  }
}
</script>

<style lang="sass">
  @import "./styles/style.scss"
</style>