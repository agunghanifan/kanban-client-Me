<template>
  <div>
    <div class="card">
      <div class="card-head">
        <h5 class="card-title">{{ dataCard.title }}</h5>
      </div>
      <div class="card-body">
        <div v-if="!isShowForm">
          <p>Description : {{ dataCard.description }}</p>
          <p>Point : {{ dataCard.point }}</p>
          <p>Assign to : {{ dataCard.assign_to }}</p>
          <p>By : {{ dataCard.User.name }}</p>
          <p>Email : {{ dataCard.User.email }}</p>
        </div>
        <div v-if="!isShowForm">
          <div>
              <div class="alert alert-danger" role="alert" v-if="showDeleteAlert">
                {{deleteError}}
              </div>
            </div>
          <button
            @click="editData"
            type="button"
            class="btn btn-warning"
            data-toggle="modal"
            :id="dataCard.id"
          >
            Edit
          </button>
          <button
            @click="deleteButton"
            type="button"
            class="btn btn-danger"
            data-toggle="modal"
            :id="dataCard.id"
          >
            delete
          </button>
          
          <!-- <button  type="button" class="btn btn-success" data-toggle="modal" :id="dataCard.id">
                Move
          </button> -->
          <!-- Button to Open the Modal -->
            <button type="button" class="btn btn-success" data-toggle="modal" :data-target="'#'+dataCard.id+'modal'">
              Changes Category
            </button>
        </div>
        <div v-if="isShowForm">
          <div v-for="(error, index) in editError" :key="index">
              <div class="alert alert-danger" role="alert" v-if="showAlert">
                {{error}}
              </div>
            </div>
          <form @submit.prevent="submitEdit">
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
              <div id="date-help" class="form-text">Give me point!</div>
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
              value="Edit task"
              id="btn-submit-addTodo"
              class="btn btn-primary"
            />
          </form>
          
          <div>
            <input
              @click.prevent="hideForm"
              type="submit"
              value="Cancel"
              id="btn-submit-addTodo"
              class="btn btn-primary"
            />  
          </div>
        </div>

        <!-- The Modal -->
            <div class="modal text-primary" :id="dataCard.id+'modal'">
              <div class="modal-dialog">
                <div class="modal-content">

                  <!-- Modal Header -->
                  <div class="modal-header">
                    <h4 class="modal-title">Pindahkan Task kemana?</h4>
                    <button type="button" class="close" data-dismiss="modal">&times;</button>
                  </div>
            
                  <!-- Modal body -->
                  <div class="modal-body">
                    <div v-if="dataNameCategory.nameCategory == 'Backlog'">
                    <button @click.prevent="move('Doing', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Doing</button>
                    <button @click.prevent="move('Done', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Done</button>
                    <button @click.prevent="move('To-Do', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">To-Do</button>
                    </div>
                    <div v-else-if="dataNameCategory.nameCategory == 'To-Do'">
                    <button @click.prevent="move('Backlog', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Backlog</button>
                    <button @click.prevent="move('Doing', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Doing</button>
                    <button @click.prevent="move('Done', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Done</button>
                    </div>
                    <div v-else-if="dataNameCategory.nameCategory == 'Doing'">
                    <button @click.prevent="move('Backlog', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Backlog</button>
                    <button @click.prevent="move('To-Do', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">To-Do</button>
                    <button @click.prevent="move('Done', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Done</button>
                    </div>
                    <div v-else-if="dataNameCategory.nameCategory == 'Done'">
                    <button @click.prevent="move('Backlog', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Backlog</button>
                    <button @click.prevent="move('To-Do', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">To-Do</button>
                    <button @click.prevent="move('Doing', dataCard.id)" type="button" class="btn btn-danger" data-dismiss="modal">Doing</button>
                    </div>
                  </div>
            
                  <!-- Modal footer -->
                  <div class="modal-footer">
                    <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                  </div>
                </div>
              </div>
            </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskCard",
  data() {
    return {
      title: "",
      description: "",
      point: "",
      assign_to: "",
      CategoryId: "",
      id: this.dataCard.id,
      isShowForm: false,
      showAlert: false,
      showDeleteAlert: false
    };
  },
  methods: {
    move(data, id) {
      this.moveData(data, id)
      this.showAlertDelete()
    },

    deleteButton() {
      this.deleteData(this.dataCard.id);
      this.showAlertDelete()
    },

    submitEdit() {
      this.submitEditData({
        id: this.dataCard.id,
        title: this.title,
        description: this.description,
        point: this.point,
        assign_to: this.assign_to,
        CategoryId: this.CategoryId,
      });
      this.showAlertTask()
    },

    showAlertDelete() {
      // console.log("masuk show alert")
      let setData = setInterval(() => {
        if(this.deleteError) {
          this.showDeleteAlert = true
        } else {
          this.showDeleteAlert = false
        }
        // console.log("selesai hitungan")
        clearInterval(setData);
      }, 2000);
      let setHide = setInterval(() => {
        this.showDeleteAlert = false
        clearInterval(setHide);
      }, 5000)
    },

    showAlertTask() {
      // console.log("masuk show alert")
      let setData = setInterval(() => {
        if(this.editError) {
          this.showAlert = true
          this.fillTheBlank()
        } else {
          this.showAlert = false
        }
        // console.log("selesai hitungan")
        clearInterval(setData);
      }, 2000);
    },

    editData() {
      this.fetchEditData(this.id);
      
      let setData = setInterval(() => {
        if(this.showDataEdit) {
          this.fillTheBlank();
        }
        clearInterval(setData);
      }, 2000);
    },

    fillTheBlank() {
      this.isShowForm = true;
      this.title = this.showDataEdit.title;
      this.description = this.showDataEdit.description;
      this.point = this.showDataEdit.point;
      this.assign_to = this.showDataEdit.assign_to;
      this.CategoryId = this.showDataEdit.CategoryId;
    },

    hideForm() {
      this.isShowForm = false;
    },
  },
  props: [
    "dataCard",
    "fetchEditData",
    "showDataEdit",
    "submitEditData",
    "deleteData",
    "loadTaskFromParent",
    "dataNameCategory",
    "moveData",
    "editError",
    "deleteError",
  ],
};
</script>

<style>
</style>