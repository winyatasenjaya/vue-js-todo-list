<template>
    <div class="container mt-2" v-if="tasks.length != 0">
      <table class="table table-striped table-responsive">
        <thead>
            <tr>
                <th scope="col">Task Title</th>
                <th>Status</th>
                <th>Created At</th>
                <th scope="col">Completed</th>
                <th scope="col">Actions</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
                <td>{{ task.subject }}</td>
                <td>{{ task.status }}</td>
                <td>{{ task.created_at }}</td>
                <td><input type="checkbox" v-model="task.completed"></td>
                <td>
                    <b-button variant="outline-secondary" class="mr-2" @click="edit(index)"> Edit </b-button>
                    <b-button variant="outline-danger" class="mr-2" @click="remove(task, index)"> Remove </b-button>
                </td>
            </tr>
        </tbody>
      </table>

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
    export default {
      name: "List",

      data() {
        return {
          tasks: [],
          taskSelected: []
        }
      },

      created() {
        /** create new task and store to local storage  */
        this.tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : [];
        console.log(this.tasks)
      },

      methods: {
        edit(index) {
          this.$router.push({ name: "form", params: { index } });
        },

        remove(task, index) {
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
        }
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