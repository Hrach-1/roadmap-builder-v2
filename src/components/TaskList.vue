<template>
  <div class="task-list" :class="{ first: firstList }" draggable="false">
    <div class="task-list-header">
      <p class="task-list-title">{{ title }}</p>
      <button @click="toggleAddTaskState" class="task-list-add-task">
        <img src="../assets/add.svg" alt="Add" width="16" />
      </button>
    </div>

    <div class="tasks" @dragenter.prevent="dragEnterTaskList">
      <div
        class="dropzone"
        :data-task-list="taskList"
        @drop.prevent="drop"
        @dragover.prevent
        @dragleave="dragLeaveTaskList"
      ></div>
      <AddTask
        v-if="addTaskState"
        :tasksList="tasks[taskList]"
        @hide="toggleAddTaskState"
        :list="taskList"
        :data-task-list="taskList"
      />
      <Task
        v-for="(task, idx) in tasks[taskList]"
        :key="task"
        :task="task"
        :list="taskList"
        :idx="idx"
        @dragItem="dragItem"
        :data-task-list="taskList"
      />
    </div>
  </div>
</template>

<script>
import Task from '@/components/Task'
import AddTask from '@/components/AddTask'

export default {
  name: 'TaskList',
  components: { Task, AddTask },
  data: () => ({
    addTaskState: false,
  }),
  props: {
    title: {
      required: true,
      type: String,
    },
    nowDrag: {
      type: Object,
      required: true,
    },
    tasks: {
      type: Array,
      required: true,
    },
    firstList: {
      default: false,
      type: Boolean,
    },
    taskList: {
      type: Number,
      required: true,
    },
  },
  methods: {
    toggleAddTaskState() {
      this.addTaskState = !this.addTaskState
    },
    dragItem(list, idx) {
      this.nowDrag.list = list
      this.nowDrag.idx = idx
    },
    dragEnterTaskList(e) {
      if (e.target.classList.contains('tasks')) {
        e.target.querySelector('.dropzone').style.display = 'block'
      } else if (
        e.target.hasAttribute('data-task-list') &&
        +e.target.dataset?.taskList !== this.nowDrag.list
      ) {
        document
          .querySelectorAll('.tasks')
          [+e.target.dataset.taskList].querySelector(
            '.dropzone'
          ).style.display = 'block'
      }
    },
    dragLeaveTaskList(e) {
      if (e.target.classList.contains('dropzone'))
        e.target.style.display = 'none'
    },
    drop(e) {
      e.target.style.display = 'none'
      this.tasks[+e.target.dataset.taskList].push(
        this.tasks[+this.nowDrag.list][+this.nowDrag.idx]
      )
      this.tasks[+this.nowDrag.list].splice(+this.nowDrag.idx, 1)
    },
  },
}
</script>

<style scoped>
.task-list.first-list {
  padding-right: 48px;
  padding-left: 0;
}

.task-list {
  width: 320px;
  border-right: 1px solid #e4e4e7;
  padding: 20px 48px;
  min-height: calc(100vh - 40px);
}

.task-list-header {
  width: 100%;
  color: #000000;
  font-weight: 500;
  align-items: center;
  display: flex;
  box-sizing: border-box;
  justify-content: space-between;
  /*outline: 1px solid blue;*/
  margin-bottom: 28px;
}

.task-list-title {
  width: 280px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  margin: 0;
}

.task-list .task-list-add-task {
  border: none;
  background: transparent;
  /*outline: 1px solid blue;*/
  cursor: pointer;
  width: 16px;
  height: 16px;
}

.task-list .task-list-add-task:focus {
  outline: none;
}

.task-list .tasks {
  height: calc(100% - 40px);
}

.tasks {
  position: relative;
}

.tasks .dropzone {
  width: 100%;
  height: 100%;
  position: absolute;
  background: rgba(238, 238, 238, 0.25);
  /*background: red;*/
  display: none;
}
</style>
