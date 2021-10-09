<template>
  <div class="task-grid">
    <template v-if="tasks.length > 0">
      <Task v-for="(task, i) in tasks"
            :key="i"
            :task="task"
            @taskDeleted="$emit('taskDeleted', i)"
            @taskCompleted="$emit('taskCompleted', i)"
            @changeName="$emit('changeName', {newName: $event, indice: i})"
      ></Task>
    </template>
    <div v-else class="no-task">Sua vida está em dia :)</div>
  </div>
</template>

<script>
import Task from "./Task";

export default {
  name: "TaskGrid",
  components: {Task},
  props: {
    tasks: {type: Array, required: true}
  }
}
</script>

<style scoped>
.task-grid {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.task-grid .task {
  margin: 10px;
}

.no-task {
  color: #aaa;
  font-size: 1.7rem;
}
</style>