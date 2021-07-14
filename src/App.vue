<template>
  <div class="roadmap">
    <div class="first-list task-list">
      <div class="task-list-header">
        <p class="task-list-title">Planned</p>
        <button @click="toggleAddTask(0)" class="task-list-add-task">
          <img src="./assets/add.svg" alt="Add" width="16">
        </button>
      </div>

      <div class="tasks" @dragover.prevent="dragOverTaskList" @dragleave="dragLeaveTaskList"
           @dragenter.prevent="dragEnterTaskList"
           @drop.prevent="drop" data-task-list="0">
        <AddTask v-if="addTask[0]" :tasksList="tasks[0]" :show="addTask[0]" @hide="toggleAddTask" list="0"/>
        <Task v-for="(task, idx) in tasks[0]" :key="task" :task="task" list="0" :idx="idx" @dragItem="dragItem"/>
      </div>
    </div>

    <div class="task-list">
      <div class="task-list-header">
        <p class="task-list-title">Under consideration</p>
        <button @click="toggleAddTask(1)" class="task-list-add-task">
          <img src="./assets/add.svg" alt="Add" width="16">
        </button>
      </div>

      <div class="tasks" @dragover.prevent="dragOverTaskList" @dragleave="dragLeaveTaskList"
           @dragenter.prevent="dragEnterTaskList"
           @drop.prevent="drop" data-task-list="1">
        <AddTask v-if="addTask[1]" :tasksList="tasks[1]" :show="addTask[1]" @hide="toggleAddTask" list="1"/>
        <Task v-for="(task, idx) in tasks[1]" :key="task" :task="task" list="1" :idx="idx" @dragItem="dragItem"/>

      </div>
    </div>

    <div class="task-list">
      <div class="task-list-header">
        <p class="task-list-title">Already Shipped</p>
        <button @click="toggleAddTask(2)" class="task-list-add-task">
          <img src="./assets/add.svg" alt="Add" width="16">
        </button>

      </div>

      <div class="tasks" @dragover.prevent="dragOverTaskList" @dragleave="dragLeaveTaskList"
           @dragenter.prevent="dragEnterTaskList"
           @drop.prevent="drop" data-task-list="2">
        <AddTask v-if="addTask[2]" :tasksList="tasks[2]" :show="addTask[2]" @hide="toggleAddTask" list="2"/>
        <Task v-for="(task, idx) in tasks[2]" :key="task" :task="task" list="2" :idx="idx" @dragItem="dragItem"/>

      </div>
    </div>
  </div>
</template>

<script>

import Task from "@/components/Task";
import TaskList from "@/components/TaskList";
import AddTask from "@/components/AddTask";

export default {
  name: 'App',
  components: {
    AddTask,
    TaskList,
    Task
  },
  data: () => ({
    addTask: [false, false, false],
    tasks: [
      [
        'Faster refresh rate on event plan',
        'Instagram stories',
        'Image optimization, minify JS/CSS/HTML',
        'Facebook groups source',
        'Shopify App'
      ],
      [
        'Web hooks for article deletion',
        'Facebook Groups Source',
        'Shopify App'
      ],
      [
        'Small resolution images',
        'Tiktok source',
        'Image optimization, minify JS/CSS/HTML',
        'Turn disconnection emails off fortnite',
        'Instagram @mention source',
        'More detailed help videos'
      ]],
    nowDrag: ''
  }),
  methods: {
    toggleAddTask(e) {
      this.addTask[e] = !this.addTask[e]
    },
    dragOverTaskList(e) {
      if (e.target.classList.contains('tasks')) e.target.style.background = 'rgba(238,238,238,0.25)'

      // e.target.style.opacity = 0.4
    },
    dragEnterTaskList(e) {
      if (e.target.classList.contains('tasks')) e.target.style.background = 'rgba(238,238,238,0.25)'


      // e.target.style.opacity = 0.4
    },
    dragLeaveTaskList(e) {
      if (e.target.classList.contains('tasks')) e.target.style.background = '#ffffff'

      // e.target.style.opacity = 1

    },
    drop(e) {
      e.target.style.background = '#ffffff'
      console.log('===================')
      console.log('Drop Task List!')
      console.log(e)
      console.log(e.target.dataset.taskList)
      this.tasks[+e.target.dataset.taskList].push(this.tasks[+this.nowDrag[0]][+this.nowDrag[1]])
      this.tasks[+this.nowDrag[0]].splice(+this.nowDrag[1], 1)
      console.log('===================')
    },
    dragItem(list, idx) {
      this.nowDrag = [list, idx]
      console.log('Now Drag : ', this.nowDrag)
    }
  }
}
</script>

<style>
body {
  font-family: 'Roboto', sans-serif;
  margin: 0;
}

.roadmap {
  margin: 0;
  padding: 0 16px;
  display: flex;
}

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

</style>
