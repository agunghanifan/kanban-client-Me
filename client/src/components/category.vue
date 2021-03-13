<template>
    <div class="container-groups" >
      <div class="card" :id="dataNameCategory.nameCategory">
        <div class="card-head" >
          <h4 class="card-title">{{ dataNameCategory.nameCategory }}</h4>
        </div>
        
        <task
          v-for="(task, index) in taskAvailable"
          :key="index"
          :dataCard="task"
          :fetchEditData="fetchEditData"
          :showDataEdit="showDataEdit"
          :submitEditData="submitEditData"
          :deleteData="deleteData"
          :loadTaskFromParent="loadTaskFromParent"
          :categoriesFromParent="categoriesFromParent"
          :dataNameCategory="dataNameCategory"
          :moveData="moveData"
          :editError="editError"
          :deleteError="deleteError"
        >
        </task>

        <div>
          <button @click="showForm" id="formAdd" v-if="!isShowForm" >Add Data</button>
        </div>
        <div v-if="isShowForm">
          <div v-for="(error, index) in addError" :key="index">
              <div class="alert alert-danger" role="alert" v-if="showAlertAdd">
                {{error}}
              </div>
            </div>
          <form @submit.prevent="addData">
            <div class="mb-3">
              <label for="title" class="form-label">Title</label>
              <input
                v-model="title"
                type="text"
                class="form-control"
                id="title"
                aria-describedby="title-help"
                placeholder="Title"
              />
              <div id="title-help" class="form-text">Type your goals</div>
            </div>
            <div class="mb-3">
              <label for="description" class="form-label">Description</label>
              <input
                v-model="description"
                type="text"
                class="form-control"
                id="description"
                aria-describedby="desc-help"
                placeholder="Description"
              />
              <div id="desc-help" class="form-text">
                Tell me more about your goals
              </div>
            </div>
            <div class="mb-3">
              <label for="due_date" class="form-label">Point</label>
              <input
                v-model="point"
                type="number"
                class="form-control"
                id="due_date"
                aria-describedby="date-help"
                placeholder="Point"
              />
              <div id="date-help" class="form-text">
                Give me point!
              </div>
            </div>
            <div class="mb-3">
              <label for="due_date" class="form-label">Assign to</label>
              <input
                v-model="assign_to"
                type="text"
                class="form-control"
                id="due_date"
                aria-describedby="date-help"
                placeholder="Assign to"
              />
              <div id="date-help" class="form-text">
                You need somebody to working this goals, right?
              </div>
            </div>
            <input
              type="submit"
              value="addTodo"
              id="btn-submit-addTodo"
              class="btn btn-primary"
            />
          </form>
          <input
              @click.prevent="hideForm"
              type="submit"
              value="Cancel"
              id="btn-submit-addTodo"
              class="btn btn-primary"
            />
        </div>
      </div>
    </div>
</template>

<script>
import task from "./task.vue";

export default {
  name: "CategoryCard",
  data() {
    return {
      title: "",
      description: "",
      point: "",
      assign_to: "",
      CategoryId: this.dataNameCategory.id,
      tasks: this.tasksFromParent,
      isShowForm: false,
      showAlertAdd: false
    };
  },
  computed: {
    taskAvailable: function () {
      let result = [];
      this.tasks.forEach((task) => {
        if (task.Category.nameCategory == this.dataNameCategory.nameCategory) {
          result.push(task);
        }
      });
      return result;
    },
  },
  methods: {
      addData() {
        this.addDataFunction({title: this.title, description: this.description, assign_to: this.assign_to, CategoryId: this.CategoryId, point: this.point})
        this.showAddAlert()
      },
      showAddAlert() {
        // console.log("masuk show alert")
        let setData = setInterval(() => {
        if(this.addError) {
          this.showAlertAdd = true
        } else {
          this.showAlertAdd = false
        }
        // console.log("selesai hitungan")
        clearInterval(setData);
      }, 2000);
      },

      showForm() {
        this.isShowForm = true;
      },
      hideForm() {
        this.isShowForm = false;
      }
  },
  components: {
    task,
  },
  props: [
    "dataNameCategory",
    "tasksFromParent",
    "addDataFunction",
    "fetchEditData",
    "showDataEdit",
    "submitEditData",
    "deleteData",
    "loadTaskFromParent",
    "categoriesFromParent",
    "dataNameCategory",
    "moveData",
    "addError",
    "deleteError",
    "editError",
    "addError"
  ],
};
</script>

<style>
</style>