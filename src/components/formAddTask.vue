<template>
      <form>

        <div class="mb-3">
          <label for="subject" class="form-label">Title</label>

          <input
            class="form-control"
            id="subject"
            v-model="formAddTask.subject"
            type="text"
            placeholder="Type your title task here"
            required
            autocomplete="off">
        </div>

        <div class="mb-3">
          <label for="status" class="form-label">Status</label>
          <select id="status" v-model="formAddTask.status" class="form-control">
            <option value="Pending">Pending</option>
            <option value="Completed">Completed</option>
          </select>
        </div>

        <div class="mb-3">
          <label for="description" class="form-label">Description</label>

          <textarea
            id="description"
            class="form-control"
            v-model="formAddTask.description"
            type="text"
            placeholder="Type your task description here"
            required
            autocomplete="off"
          ></textarea >
        </div>

        <button class="btn btn-primary" type="submit" variant="btn-primary" @click="saveTask"> Save </button>
      </form>
</template>

<script>
import ToastMixin from "@/mixins/toastMixin.js";

export default {
  name: "formAddTask",

  mixins: [ToastMixin],

  data() {
    return {
      formAddTask: {
        subject: "",
        created_at: new Date().toLocaleString(),
        status: "",
        completed: false,
        description: ""
      },
      methodSave: "new"
    }
  },

  created() {
    if(this.$route.params.index === 0 || this.$route.params.index !== undefined){
      this.methodSave = "update";
      let tasks = JSON.parse(localStorage.getItem("tasks"));
      this.formAddTask = tasks[this.$route.params.index];
    }
  },

  methods: {
    saveTask() {
      console.log('save task ')
      console.log(this.formAddTask)

      this.formAddTask.completed = this.formAddTask.status == 'Completed' ? true : false;

      if(this.methodSave === "update"){
        /** update task list */
        let tasks = JSON.parse(localStorage.getItem("tasks"));
        tasks[this.$route.params.index] = this.formAddTask;
        localStorage.setItem("tasks", JSON.stringify(tasks));
        this.showToast("success", "Success!", "Task List updated success!");
        this.$router.push({ name: "list" });
        return;
      }
      /** Load task list data */
      let tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : [];

      /** prepare save data */
      this.formAddTask.created_at = new Date().toLocaleString();
      tasks.push(this.formAddTask);

      /** store data to local storage */
      localStorage.setItem("tasks", JSON.stringify(tasks));
      this.showToast("success", "Success!", "Task List create Success!");
      this.$router.push({ name: "list" });
    }
  }
}
</script>

<style>
  .container{
    height: calc(100vh - 64px);
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    padding-top: 30px;
  }
  .texto{
    /* color: rgb(219, 209, 209); */
    font-weight: 600;
  }
  #description, #subject{
    background-color: #fff;
    border-radius: 7px;
    /* color: rgb(255, 255, 255); */
    /* box-shadow: -1px 1px 11px 1px rgba(0, 0, 0, 0.36);
    -webkit-box-shadow: -1px 1px 11px 1px rgba(0, 0, 0, 0.36); */
    /* border: none; */
}
</style>

