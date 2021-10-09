<template>
  <div id="app">
    <h1>Tarefas</h1>
        <TaskProgress :progress="progress"/>
    <NewTask @taskAdded="addTask"></NewTask>
    <TaskGrid
        :tasks="tasks"
        @taskDeleted="removeTask($event)"
        @taskCompleted="toggleStatus($event)"
        @changeName="newName($event)"
    ></TaskGrid>

  </div>
</template>

<script>
import TaskGrid from "./components/TaskGrid";
import NewTask from "./components/NewTask";
import TaskProgress from "./components/TaskProgress";

export default {
  components: {TaskGrid, NewTask, TaskProgress},
  data() {
    return {
      tasks: [
        // {name: 'Lavar a louça', pending: true},
        // {name: 'Fazer dever', pending: false},
      ]
    }
  },
  methods: {
    addTask(task) {
      const reallyNew = this.tasks.filter(t => t.name === task.name);
      if (reallyNew.length > 0) return
      this.tasks.push({
        name: task.name, pending: true
      })
    },
    toggleStatus(i) {
      this.tasks[i].pending = !this.tasks[i].pending;
    },
    removeTask(indice) {
      this.tasks.splice(indice, 1)
    },
    newName(obj){
      this.tasks[obj.indice].name = obj.newName
    }
  },
  computed: {
    progress() {
      const total = this.tasks.length
      let completed = this.tasks.filter(t => t.pending === false).length
      return Math.round(completed / total * 100) || 0
    }
  },
  watch: {
    tasks: {
      deep: true,
      handler() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
      }
    }
  },
  created() {
    const json = localStorage.getItem('tasks')
    this.tasks = JSON.parse(json) || []
  }
}
</script>

<style>
body {
  font-family: 'Lato', sans-serif;
  background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
  color: #FFF;
}

#app {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app h1 {
  margin-bottom: 5px;
  font-weight: 300;
  font-size: 3rem;
}
</style>
