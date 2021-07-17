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
        @dragover.prevent='dragOver'
        @dragleave="dragLeaveTaskList"
      ></div>

      <div class="add-card-block">
        <ButtonBigAdd v-show="!addTaskState" @click="toggleAddTaskState" label="Add a card"/>
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
import ButtonBigAdd from "@/components/ButtonBigAdd";

export default {
  name: 'TaskList',
  components: {ButtonBigAdd, Task, AddTask},
  data: () => ({
    addTaskState: false,
    dragPlace: 0
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
        e.target.hasAttribute('data-task-list')
      ) {
        document.querySelectorAll('.dropzone').forEach((el) => {
          el.style.display = 'none'
        })
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
      this.tasks[+e.target.dataset.taskList].splice(this.place, 0, this.tasks[this.nowDrag.list][this.nowDrag.idx])

      if (+e.target.dataset.taskList === this.nowDrag.list){
        if (this.nowDrag.idx > this.place) {
          this.tasks[this.nowDrag.list].splice(this.nowDrag.idx+1, 1)
        }else if (this.nowDrag.idx < this.place) {
          this.tasks[this.nowDrag.list].splice(this.nowDrag.idx, 1)
        }
      } else {
        this.tasks[this.nowDrag.list].splice(this.nowDrag.idx, 1)
      }


      this.tasks.forEach((el) => {
        el.filter((task) => task !== '')
      })
      document.querySelectorAll('.dropzone').forEach((el) => {
        el.style.display = 'none'
      })
    },
    dragOver(e) {
      const between = 88
      const startWith = 156
      const cordY = e.clientY
      this.place = this.tasks[this.taskList].length
      if (cordY < startWith + 44) {
        this.place = 0
      } else {
        for (let i = 0; i < this.tasks[this.taskList].length; i++) {
          if (cordY > startWith + 44 + between * i && cordY < startWith + 44 + between * (i + 1)) {
            this.place = (i + 1)
            break
          }
        }
      }
    }
    ,
    saveTaskEdit(obj) {
      this.tasks[obj.list].splice(obj.idx, 1, obj.text)
    }
    ,
    deleteTask(obj) {
      this.tasks[obj.list].splice(obj.idx, 1)
    }
  },
}
</script>

<style scoped lang="scss">
.task-list {
  border-right: 1px solid #e4e4e7;
  padding: 0 32px;
  min-height: calc(100%);

  &.first-list {
    padding-right: 48px;
    padding-left: 0;
  }

  .task-list-header {
    width: 100%;
    color: #000000;
    font-weight: 500;
    align-items: center;
    display: flex;
    box-sizing: border-box;
    justify-content: space-between;
    margin-bottom: 28px;

    .task-list-title {
      width: 280px;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      margin: 0;
    }

    .task-list-add-task {
      border: none;
      background: transparent;
      cursor: pointer;
      width: 16px;
      height: 16px;

      &:focus {
        outline: none;
      }
    }
  }

  .tasks {
    min-height: calc(100% - 40px - 40px);
    position: relative;
    padding: 16px;
    width: 322px;

    .dropzone {
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

    .add-card-block {
      margin-bottom: 24px;
    }
  }
}
</style>
