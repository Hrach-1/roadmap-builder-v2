<template>

  <div
    class="task-box"
    draggable="true"
    @dragstart="dragStart"
    @dragend="dragEnd"
    @drag="drag"
    :class="{
        first: idx === 0
      }"
  >
    <p class="task" :data-task-list="list">{{ task }}</p>
    <div class="options">
      <img
        src="../assets/more.svg"
        alt="more"
        width="16"
        draggable="false"
        :data-task-list="list"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Task',
  data: () => ({}),
  props: {
    task: {
      require: true,
      type: String,
    },
    list: Number,
    idx: Number,
  },
  inject: ['nowDrag'],
  methods: {
    dragStart(e) {
      e.target.style.opacity = 0.4
      this.nowDrag.list = this.list
      this.nowDrag.idx = this.idx
    },
    dragEnd(e) {
      e.target.style.opacity = 1
    },
    drag(e) {
      console.log(e)
    },
  },
}
</script>

<style scoped>
.task-box {
  border: 1px solid #e4e4e4;
  padding: 20px 16px;
  border-radius: 4px;
  width: 322px;
  color: #868f9c;
  font-weight: normal;
  display: flex;
  justify-content: space-between;
  align-items: center;
  /* cursor: grab; */
  box-shadow: 0.5px 2px 14px 0px #e4e4e4;
  transition: 0.2s;
  box-sizing: border-box;
  background: #ffffff;
  margin-top: 24px;

}

.first {
  margin-top: 0;
}

.task-box:hover {
  box-shadow: 6px 10px 11px 0px #e4e4e4;
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

.task-box-container {
  position: relative;
  /*background: red;*/
  border-radius: 6px;
}

.task-box-container .task-box-back {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: blue;
  border-radius: 6px;
  z-index: -1;
}
</style>
