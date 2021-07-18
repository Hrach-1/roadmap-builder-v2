<template>
  <div
    class="task-list"
    :class="{ 'first-list': taskList === 0 }"
    draggable="false"
  >
    <div class="task-list-header">
      <div class="task-list-title">
        <h3 v-if="!taskListTitleEdit">{{ titles[taskList] }}</h3>
        <div class="task-list-title-edit" v-if="taskListTitleEdit">
          <input type="text" ref="taskListTitle" v-model="taskListTitle" @keypress.enter="taskListTitleSave">
          <EditToolbar @toolbarSave="taskListTitleSave" @toolbarClose="toggleTaskListTitleEdit"/>
        </div>
      </div>
      <div class="options">
        <ButtonMore @click="toggleTaskListTooltip"/>
        <Tooltip :add="true" v-if="taskListTooltipShow" @edit="toggleTaskListTitleEdit" @add="toggleAddTaskState"
                 @delete="taskListDelete"/>
      </div>
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

      <div
        class="task-block"
        v-for="(task, idx) in tasks[taskList]"
        :ref="tasksList"
      >
        <Task
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
  </div>
</template>

<script>
import Task from '@/components/Task'
import AddTask from '@/components/AddTask'
import ButtonBigAdd from "@/components/ButtonBigAdd";
import ButtonMore from "@/components/ButtonMore";
import Tooltip from "@/components/Tooltip";
import EditToolbar from "@/components/EditToolbar";

export default {
  name: 'TaskList',
  components: {EditToolbar, Tooltip, ButtonMore, ButtonBigAdd, Task, AddTask},
  data: () => ({
    addTaskState: false,
    taskListTooltipShow: false,
    taskListTitleEdit: false,
    dragPlace: null,
    taskListTitle: '',
    taskRefs: [],
    dragPlaceOld: null
  }),
  mounted() {
    this.taskListTitle = this.titles[this.taskList]
  },
  beforeUpdate() {
    this.taskRefs = []
  },
  inject: ['nowDrag'],
  props: {
    titles: {
      required: true,
      type: Array,
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
    tasksList(el) {
      this.taskRefs.push(el)
    },
    toggleAddTaskState() {
      this.addTaskState = !this.addTaskState
      this.$refs.addTask.focus()
      this.taskListTooltipShow = false
    },
    toggleTaskListTooltip() {
      this.taskListTooltipShow = !this.taskListTooltipShow
    },
    toggleTaskListTitleEdit() {
      this.taskListTitleEdit = !this.taskListTitleEdit
      this.toggleTaskListTooltip()
    },
    dragEnterTaskList(e) {
      document.querySelectorAll('.dropzone').forEach((el) => {
        el.style.display = 'none'
      })
      document.querySelectorAll('.task-block').forEach(el => {
        el.style.cssText = 'margin-top: 24px;'
      })

      if (e.target.hasAttribute('data-task-list')) {
        // document.querySelectorAll('.dropzone').forEach((el) => {
        //   el.style.display = 'none'
        // })
        document.querySelectorAll('.tasks')[+e.target.dataset.taskList].querySelector('.dropzone')
          .style.display = 'block'
      }
    },
    dragLeaveTaskList(e) {
      document.querySelectorAll('.dropzone').forEach((el) => {
        el.style.display = 'none'
      })
    },
    drop(e) {
      this.tasks[+e.target.dataset.taskList].splice(this.dragPlace, 0, this.tasks[this.nowDrag.list][this.nowDrag.idx])

      if (+e.target.dataset.taskList === this.nowDrag.list) {
        if (this.nowDrag.idx > this.dragPlace) {
          this.tasks[this.nowDrag.list].splice(this.nowDrag.idx + 1, 1)
        } else if (this.nowDrag.idx < this.dragPlace) {
          this.tasks[this.nowDrag.list].splice(this.nowDrag.idx, 1)
        }
      } else {
        this.tasks[this.nowDrag.list].splice(this.nowDrag.idx, 1)
      }

      this.taskRefs.forEach(el => {
        el.style.cssText = 'margin-top: 24px;'
      })
      this.tasks.forEach((el) => {
        el.filter((task) => task !== '')
      })
      document.querySelectorAll('.dropzone').forEach((el) => {
        el.style.display = 'none'
      })
    },
    dragOver(e) {
      const between = 88,
        betweenFirst = 44,
        bigBetween = 176,
        bigBetweenFirst = 132

      const startWith = 204
      const cordY = e.clientY

      let area = startWith
      const areas = [area]

      if (this.dragPlace) {
        for (let i = 0; i < this.tasks[this.taskList].length; i++) {
          if (this.dragPlace === i && i === 0) {
            area += bigBetweenFirst
            areas.push(area)
          } else if (this.dragPlace === i) {
            area += bigBetween
            areas.push(area)
          }
          else if(i === 0){
            area += betweenFirst
            areas.push(area)
          } else {
            area += between
            areas.push(area)
          }
        }
      }

      this.dragPlace = this.tasks[this.taskList].length
      if (cordY < startWith + 44) {
        this.dragPlace = 0
      } else {
        for (let i = 0; i < this.tasks[this.taskList].length; i++) {
          if (cordY > startWith + 44 + between * i && cordY < startWith + 44 + between * (i + 1)) {
            this.dragPlace = (i + 1)
            break
          }
        }
      }
      for (let i = 0; i < this.tasks[this.taskList].length; i++) {
        if(cordY >= areas[i] && cordY <= areas[i+1]) {
          this.dragPlace = i
        }
      }



      if (this.dragPlace !== this.dragPlaceOld && this.dragPlaceOld) {
        this.taskRefs[this.dragPlaceOld].style.cssText = 'margin-top: 24px;'
        this.dragPlaceOld = this.dragPlace
      } else if (this.dragPlace !== this.tasks[this.taskList].length) {
        this.taskRefs[this.dragPlace].style.cssText = 'margin-top: 112px;'
      }

      this.taskRefs.forEach(el => {
        el.style.cssText = 'margin-top: 24px;'
      })
      if (this.dragPlace !== this.tasks[this.taskList].length) {
        this.taskRefs[this.dragPlace].style.cssText =  'margin-top: 112px;'
      }
    }
    ,
    saveTaskEdit(obj) {
      this.tasks[obj.list].splice(obj.idx, 1, obj.text)
    }
    ,
    deleteTask(obj) {
      this.tasks[obj.list].splice(obj.idx, 1)
    },
    taskListTitleSave() {
      this.titles.splice(this.taskList, 1, this.taskListTitle)
      this.toggleTaskListTitleEdit()
      this.toggleTaskListTooltip()
    },
    taskListDelete() {
      this.tasks.splice(this.taskList, 1)
      this.titles.splice(this.taskList, 1)
      this.taskListTitle = this.titles[this.taskList]
      this.toggleTaskListTooltip()
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
    height: 64px;

    .options {
      position: relative;

    }

    .task-list-title {
      width: 294px;
      overflow: hidden;
      text-overflow: ellipsis;
      margin: 0;
      margin-right: 16px;

      h3 {
        margin: 0;
        padding: 0;
        text-overflow: ellipsis;
        width: 100%;
      }

      input {
        width: 100%;
        padding: 12px 16px;
        font-size: 16px;
        margin: 0;
        box-sizing: border-box;
        margin-top: 40px;
        border: 1px solid #e4e4e4;
        border-radius: 4px;

      }
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

    .task {
      margin-top: 24px;
    }
  }
}
</style>
