<template>
  <taskEditor/>
</template>

<script>
  import taskEditor from "../components/taskEditor.vue"

  export default {

    name: "List",
    components: {
      taskEditor,
    },

    data() {
      return {
        tasks: [],
        newTask: '',
        status: 'Pending',
        editedTask: null,
        editedTaskIndex: -1
      }
    },

    methods: {
        addTask() {
            if (this.newTask.trim() === '') return;

            const task = {
                subject: this.newTask,
                status: this.status,
                created_at: new Date().toLocaleString(),
                completed: false
            };

            this.tasks.push(task);
            this.newTask = '';
        },
        editTask(task, index) {
            this.editedTask = { ...task };
            this.editedTaskIndex = index;
        },
        updateTask() {
            if (this.editedTaskIndex !== -1) {
                this.tasks.splice(this.editedTaskIndex, 1, this.editedTask);
                this.cancelEdit();
            }
        },
        deleteTask(index) {
            this.tasks.splice(index, 1);
        },
        cancelEdit() {
            this.editedTask = null;
            this.editedTaskIndex = -1;
        }
    }

  }
</script>
