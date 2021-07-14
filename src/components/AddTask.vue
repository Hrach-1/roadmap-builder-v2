<template>
  <div class="add-task">
    <textarea id="add-task-first" v-model="task" ref="task"></textarea>

    <button type="button" @click="addTask">Add Task</button>
  </div>
</template>

<script>
export default {
  name: 'AddTask',
  props: ['tasksList', 'list'],
  data: () => ({
    task: '',
  }),
  methods: {
    addTask(e) {
      if (this.task) this.tasksList.push(this.task)
      this.task = ''
      this.$emit('hide', this.list)
    },
  },
}
</script>

<style scoped>
.add-task {
  overflow: auto;
}

.add-task textarea {
  font-size: 18px;
  width: 320px;
  height: 60px;
  border: 1px solid #e4e4e7;
  border-radius: 4px;
  color: #868f9c;
  font-weight: normal;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  box-shadow: 0.5px 2px 14px 0px #e4e4e4;
  transition: 0.2s;
  box-sizing: border-box;
  resize: none;
}

.add-task textarea:focus {
  outline: none;
}

.add-task button {
  margin-top: 8px;
  border: 1px solid #e4e4e7;
  border-radius: 8px;
  background: transparent;
  padding: 8px 8px;
  color: #868f9c;
  float: right;
  cursor: pointer;
  transition: 0.2s;
}

.add-task button:hover {
  background: #f3f3f3;
}
</style>
