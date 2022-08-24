<template>
  <div>
    <AddTask v-show="showAddTask" @add-task="addTask" />
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import AddTask from "../components/AddTask.vue";
import Tasks from "../components/Tasks.vue";

export default {
  name: "Home",
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  props: {
    showAddTask: Boolean,
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
  methods: {
    async deleteTask(id) {
      if (confirm("Are you sure!")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
          headers: {
            "Content-type": "application/json",
          },
        });

        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error deleting the task");
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = await {
        ...taskToToggle,
        reminder: !taskToToggle.reminder,
      };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });

      res.status === 200
        ? (this.tasks = this.tasks.map((task) =>
            task.id === id ? { ...task, reminder: !task.reminder } : task
          ))
        : alert("Error updating the task");
    },
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async fetchTasks() {
      const res = await fetch("api/tasks");

      const data = await res.json();

      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);

      const data = await res.json();

      return data;
    },
  },
};
</script>

<style>
</style>