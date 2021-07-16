<template>
  <div class="add-task" @keypress.enter="addTask">
    <textarea v-model="task" ref="task" rows="1" cols="27"></textarea>

    <div class="toolbar">
      <button type="button" @click="close" class="add-task-button-close">
        <span class="material-icons">close</span>
      </button>
      <button type="button" @click="addTask" class="add-task-button-add">Add Task</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddTask',
  props: ['tasksList', 'list', 'addTaskState'],
  data: () => ({
    task: '',
  }),
  methods: {
    addTask(e) {
      if (this.task) this.tasksList.push(this.task)
      this.task = ''
      this.$refs.task.rows = 1
      this.$emit('hide', this.list)
    },
    focus() {
      setTimeout(() => this.$refs.task.focus(), 10)
    },
    close() {
      this.$emit('hide', this.list)
    }
  },
  watch: {
    task(val, oldVal) {
      if ((val.length - 1) !== 0) {
        if ((val.length - 1) % 29 === 0 && val.length > oldVal.length) this.$refs.task.rows = +this.$refs.task.rows + 1
        else if ((val.length) % 29 === 0 && val.length < oldVal.length) this.$refs.task.rows = +this.$refs.task.rows - 1
      }
    }
  }
}
</script>

<style scoped>
.add-task {
  overflow: auto;
  margin-bottom: 24px;
}

.add-task textarea {
  /*margin: auto;*/
  font-size: 18px;
  /*width: 320px;*/
  /*height: 100px;*/
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
  padding: 20px 16px;

}

.add-task textarea:focus {
  outline: none;
}

.add-task .add-task-button-add {
  border: 1px solid #e4e4e7;
  border-radius: 8px;
  background: transparent;
  padding: 8px 8px;
  color: #868f9c;
  cursor: pointer;
  transition: 0.2s;
}

.add-task .add-task-button-add:hover {
  background: #f3f3f3;
}

.add-task-button-close {
  border: none;
  background: transparent;
  margin-right: 16px;
  cursor: pointer;
}

.add-task-button-close:hover span {
  color: #000000;
}

.add-task-button-close span {
  color: #868f9c;
  font-size: 28px;
  transition: 0.2s;
}

.toolbar {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin-top: 16px;
}
</style>
