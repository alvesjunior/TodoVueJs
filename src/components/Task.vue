<template>
  <div class="task"
       @click="$emit('taskCompleted', task)"
       :class="stateClass">
    <span v-if="task.pending === true" class="edit" @click.stop="showInput = !showInput">
      <v-icon scale="1" name="edit"/>
    </span>
    <span @click.stop="$emit('taskDeleted', task)" class="close">x</span>
    <!-- stacked icons -->
    {{ task.name }}

      <input
          v-if="task.pending === true && showInput === true"
          @click.stop=""
          class="input-edit"
          type="text"
          v-model="newName"
          @keydown.enter="changeName"
      >


  </div>

</template>
<script>
export default {
  name: "Task",
  props: ['task'],
  data() {
    return {
      showInput: false,
      newName: ''
    }
  },
  computed: {
    stateClass() {
      if (this.task.pending) {
        return 'pending'
      } else {
        return 'done'
      }
    }
  },
  methods: {
    changeName(){
      this.$emit('changeName', this.newName)
      this.newName = ''
      this.showInput = false
    }

  }
}
</script>

<style scoped>
.task {
  box-sizing: border-box;
  width: 350px;
  height: 150px;
  padding: 10px;
  border-radius: 8px;
  font-size: 2rem;
  cursor: pointer;
  user-select: none;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
}

.pending {
  border-left: 12px solid #b73229;
  background-color: #f44336;
}

.done {
  color: #DDDDDD;
  border-left: 12px solid #0a8f08;
  background-color: #4caf50;
  text-decoration: line-through;
}

.pending .close {
  background-color: #b73229;
}

.done .close {
  background-color: #0a8f08;
}

.close {
  position: absolute;
  right: 10px;
  top: 10px;
  font-size: 0.9rem;
  height: 20px;
  width: 20px;
  border-radius: 10px;
  display: flex;
  justify-content: center;
}

.edit {
  position: absolute;
  right: 10px;
  bottom: 10px;
  font-size: 0.9rem;
  height: 20px;
  width: 20px;
  border-radius: 10px;
  display: flex;
  justify-content: center;
}

.input-edit {
  width: 80%;
  background: #00000042;
  border: 1px;
  border-radius: 5px;
  font-size: 1.5rem;
  margin-top: 5px;
  color: #fff;
  padding: 5px 0 5px 10px;
}

</style>