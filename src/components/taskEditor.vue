<template>
    <div class="container mt-2" v-if="tasks.length != 0">
      <table class="table table-striped table-responsive">
        <thead>
            <tr>
                <th scope="col">Subject Task</th>
                <th>Status</th>
                <th>Created At</th>
                <th scope="col">Completed</th>
                <th scope="col">Actions</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
                <td>
                  <s v-if="task.status=='Completed'">{{ task.subject }}</s>
                  <div v-if="task.status=='Pending'">{{ task.subject }}</div>
                </td>
                <td>{{ task.status }}</td>
                <td>
                  <s v-if="task.status=='Completed'">{{ task.created_at }}</s>
                  <div v-if="task.status=='Pending'">{{ task.created_at }}</div>
                </td>
                <td><input type="checkbox" v-model="task.completed" @click="completedTask($event, task, index)" ></td>
                <td>
                    <b-button variant="outline-secondary" class="mr-2" @click="edit(index)"> Edit </b-button>
                    <b-button variant="outline-danger" class="mr-2" @click="remove(task, index)"> Remove </b-button>
                </td>
            </tr>
        </tbody>
      </table>

      <b-modal ref="modalRemove" hide-footer title="Confirmation">
        <div class="d-block text-center">
          <p>Are you sure?</p>
          <p>Task: {{ taskSelected.subject }}</p>
        </div>
        <div class="mt-3 d-flex justify-content-end">
          <b-button variant="outline-secondary" class="mr-2" @click="hideModal"> Cancel </b-button>
          <b-button variant="outline-danger" class="mr-2" @click="confirmRemoveTask"> Remove </b-button>
        </div>
      </b-modal>
    </div>

    <div class="container mt-2 alinhar" v-else>
      <b-card class="mb-2" id="card_info">
        <b-card-text>You dont have any Task.</b-card-text>
        <b-card-text>Do you want to make a new task?</b-card-text>
        <b-button variant="outline-primary" class="mr-2" to="/form">Create New Task</b-button>
      </b-card>
    </div>
</template>

    <script>

    import ToastMixin from "@/mixins/toastMixin.js";

    export default {
      name: "List",
      mixins: [ToastMixin],

      data() {
        return {
          tasks: [],
          taskSelected: []
        }
      },

      created() {
        /** create new task and store to local storage  */
        this.tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : [];
      },

      methods: {
        edit(index) {
          this.$router.push({ name: "form", params: { index } });
        },

        remove(task, index) {
          /** remove task from list */
          this.taskSelected = task;
          this.taskSelected.index = index;
          this.$refs.modalRemove.show();
        },

        hideModal() {
          this.$refs.modalRemove.hide();
        },

        confirmRemoveTask() {
          this.tasks.splice(this.taskSelected.index, 1);
          localStorage.setItem("tasks", JSON.stringify(this.tasks));
          this.hideModal();
        },

        completedTask(event, task, index) {
          /** get tasklist */
          let tasks = JSON.parse(localStorage.getItem("tasks"));

          tasks[index]['completed'] = (event.target.checked == true) ? true : false;
          tasks[index]['status'] = (event.target.checked == true) ? 'Completed' : 'Pending';

          localStorage.setItem("tasks", JSON.stringify(tasks));
          this.tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : [];

          this.showToast("success", "Success!", "Task List updated success!");
        },
      }
    }
    </script>

    <style>
      #card_info{
      background-color: #fff;
      border-radius: 20px;
      color: gray;
      box-shadow: -1px 1px 11px 1px rgba(0,0,0,0.36);
      -webkit-box-shadow: -1px 1px 11px 1px rgba(0,0,0,0.36);
      }
      .alinhar{
        height: calc(100vh - 64px) !important;
        width: 100vh !important;
        display: flex !important;
        justify-content: flex-start !important;
        align-items: center !important;
      }
    </style>