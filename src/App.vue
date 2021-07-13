<template>
  <div class="roadmap">
    <div class="first-list task-list">
      <div class="task-list-header">
        <p class="task-list-title">Planned</p>
        <div class="task-list-options">
          <img src="./assets/more.svg" alt="more" width="16">
        </div>
      </div>

      <div class="tasks">
        <Task task="Faster refresh rate on event plan"/>
      </div>
    </div>

    <div class="task-list">
      <div class="task-list-header">
        <p class="task-list-title">Under consideration</p>
        <div class="task-list-options">
          <img src="./assets/more.svg" alt="more" width="16">
        </div>
      </div>

      <div class="tasks">
        <Task task="Web hook for article deletion"/>
      </div>
    </div>

    <div class="task-list">
      <div class="task-list-header">
        <p class="task-list-title">Already Shipped</p>
        <div class="task-list-options">
          <img src="./assets/more.svg" alt="more" width="16">
        </div>
      </div>

      <div class="tasks">
        <Task task="Small resolution images"/>
      </div>
    </div>
  </div>
</template>

<script>

import Task from "@/components/Task";
import TaskList from "@/components/TaskList";
export default {
  name: 'App',
  components: {
    TaskList,
    Task
  }
}
</script>

<style>
body {
  font-family: 'Roboto', sans-serif;
  margin: 0;
}

.roadmap{
  margin: 0;
  padding: 0 16px;
}

.task-list.first-list {
  padding-right: 48px;
  padding-left: 0;
}

.task-list {
  width: 320px;
  border-right: 1px solid #e4e4e7;
  min-height: calc(100vh - 40px);
  padding: 20px 48px;
  display: inline-block;
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

</style>
