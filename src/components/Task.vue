<template>
  <div class="task-box" draggable="true" @dragstart="dragStart" @dragend="dragEnd">
    <p class="task" :data-task-list="list">{{task}}</p>
    <div class="options">
      <img src="../assets/more.svg" alt="more" width="16" draggable="false" :data-task-list="list">
    </div>
  </div>
</template>

<script>
export default {
  name: "Task",
  data: () => ({
  }),
  props: {
    task: {
      require: true,
      type: String,
    },
    list: Number,
    idx: Number
  },
  methods: {
    dragStart(e) {
      console.log('Drag start')
      // e.dataTransfer.setData('text/plain', null)
      e.target.style.opacity = 0.4
      this.$emit('dragItem', this.list, this.idx)
    },
    dragEnd(e) {
      console.log('Drag End')
      e.target.style.opacity = 1
    },
  }
}
</script>

<style scoped>
.task-box {
  border: 1px solid #e4e4e7;
  padding: 20px 16px;
  border-radius: 4px;
  width: 320px;
  color: #868f9c;
  font-weight: normal;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  box-shadow: 0.5px 2px 14px 0px #e4e4e4;
  transition: 0.2s;
  box-sizing: border-box;
  margin-top: 24px;
  background: #ffffff;
}

.task-box:hover {
  box-shadow: 1px 8px 14px 0px #e4e4e4;
}

.task-box:hover .options {
  opacity: 1;
}

.task-box .options {
  transition: 0.2s;
  opacity: 0;
}

.task {
  width: 260px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  margin: 0;
}


</style>