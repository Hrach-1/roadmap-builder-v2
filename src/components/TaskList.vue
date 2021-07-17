<template>
  <div
    class="task-list"
    :class="{ 'first-list': taskList === 0 }"
    draggable="false"
  >
    <div class="task-list-header">
      <p class="task-list-title">{{ title }}</p>
      <button @click="toggleAddTaskState" class="task-list-add-task">
        <img src="../assets/add.svg" alt="Add" width="16"/>
      </button>
    </div>

    <div
      class="tasks"
      @dragover.prevent
      @dragenter.prevent="dragEnterTaskList"
      @drop.prevent
      :data-task-list="taskList"
      draggable="false"
    >
      <div
        class="dropzone"
        :data-task-list="taskList"
        @drop.prevent="drop"
        @dragover.prevent
        @dragleave="dragLeaveTaskList"
      ></div>

      <div class="add-card-block">
        <div class="show-add-card" v-show="!addTaskState" @click="toggleAddTaskState">
          <button><span class="material-icons">add</span>Add a card</button>
        </div>
        <AddTask
          v-show="addTaskState"
          :tasksList="tasks[taskList]"
          @hide="toggleAddTaskState"
          :list="taskList"
          :data-task-list="taskList"
          ref="addTask"
        />
      </div>

      <Task
        v-for="(task, idx) in tasks[taskList]"
        :key="idx"
        :task="task"
        :list="taskList"
        :idx="idx"
        :data-task-list="taskList"
        @editTask="saveTaskEdit"
        @deleteTask="deleteTask"
      />
    </div>
  </div>
</template>

<script>
import Task from '@/components/Task'
import AddTask from '@/components/AddTask'

export default {
  name: 'TaskList',
  components: {Task, AddTask},
  data: () => ({
    addTaskState: false,
  }),
  inject: ['nowDrag'],
  props: {
    title: {
      required: true,
      type: String,
    },
    tasks: {
      type: Array,
      required: true,
    },
    taskList: {
      type: Number,
      required: true,
    },
  },
  methods: {
    toggleAddTaskState() {
      this.addTaskState = !this.addTaskState
      this.$refs.addTask.focus()
    },
    dragEnterTaskList(e) {
      if (
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
      document.querySelectorAll('.dropzone').forEach((el) => {
        el.style.display = 'none'
      })
      this.tasks[+e.target.dataset.taskList].push(
        this.tasks[this.nowDrag.list][this.nowDrag.idx]
      )
      this.tasks[this.nowDrag.list].splice(this.nowDrag.idx, 1)
      this.tasks.forEach((el) => {
        el.filter((el) => el !== el)
      })
    },
    saveTaskEdit(obj) {
      this.tasks[obj.list].splice(obj.idx, 1, obj.text)
    },
    deleteTask(obj) {
      this.tasks[obj.list].splice(obj.idx, 1)
    }
  },
}
</script>

<style scoped lang="scss">
.task-list.first-list {
  padding-right: 48px;
  padding-left: 0;
}

.task-list {
  border-right: 1px solid #e4e4e7;
  padding: 0 32px;
  min-height: calc(100%);
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
  height: calc(100% - 40px - 40px);
  position: relative;
  padding: 16px;
  width: 322px;
}

.tasks .dropzone {
  width: 100%;
  height: 100%;
  position: absolute;
  border-radius: 8px;
  background: rgba(226, 226, 226, 0.25);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: none;
}

.show-add-card {
  height: 64px;
  display: flex;
  //outline: navajowhite 2px solid;
  margin-bottom: 24px;
  button {
    color: #868f9c;
    border: none;
    /*background: white;*/
    //background: transparent;
    font-size: 16px;
    font-family: 'Roboto', sans-serif;
    cursor: pointer;
    width: 100%;
    border-radius: 8px;
    //outline: #add5ff 2px solid;
    display: flex;
    justify-content: center;
    align-items: center;
   background: rgb(238, 238, 238);
    transition: 0.2s background-color;
    &:hover {
      background: rgba(238, 238, 238, 0.8);

    }
    span {
      //outline: #edadff 2px solid;
      margin-right: 8px;
    }
  }
}
</style>
