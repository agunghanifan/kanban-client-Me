<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Kanban Boards</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
        </div>
      </div>
      <form class="d-flex">
        <input
          @click.prevent="logOut"
          type="submit"
          class="btn btn-primary"
          id="btn-logout"
          value="Log Out"
        />
      </form>
    </nav>
    <h1 style="text-align: center">{{ greetings }}</h1>
    
    <div class="container-card">
      <categoryBoard
        v-for="(category, index) in categoriesFromParent"
        :key="index"
        :dataNameCategory="category"
        :tasksFromParent="tasksFromParent"
        :addDataFunction="addData"
        :fetchEditData="fetchEditData"
        :showDataEdit="showDataEdit"
        :submitEditData="submitEditData"
        :deleteData="deleteData"
        :loadTaskFromParent="loadTaskFromParent"
        :categoriesFromParent="categoriesFromParent"
        :moveData="moveData"
        :editError="editError"
        :deleteError="deleteError"
        :addError="addError"
      >
    </categoryBoard>
    </div>
  </div>
</template>

<script>
import categoryBoard from "../components/category.vue";

export default {
  name: "kanbanBoard",
  data() {
    return {
      index: [],
      task: [],
    };
  },
  methods: {
    logOut() {
      this.$emit("logOut");
    },
  },
  components: {
    categoryBoard,
  },
  props: [
    "loadTaskFromParent",
    "tasksFromParent",
    "greetings",
    "categoriesFromParent",
    "addData",
    "fetchEditData",
    "showDataEdit",
    "submitEditData",
    "deleteData",
    "moveData",
    "editError",
    "deleteError",
    "addError"
  ],
};
</script>

<style>
</style>