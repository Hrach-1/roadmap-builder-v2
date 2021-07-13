<template>
  <div class="task-list">
    <div class="header">
      <p class="task">{{ title }}</p>
      <div class="options">
        <img src="../assets/more.svg" alt="more" width="16">
      </div>
    </div>
    <div class="tasks">
      <Task task="Faster refresh rate on event plan"/>
    </div>
  </div>
</template>

<script>
import Task from "@/components/Task";
export default {
  name: "TaskList",
  components: {Task},
  props: {
    title: {
      required: true,
      type: String
    }
  }
}
</script>

<style scoped>
.task-list {
  width: 320px;
  border-right: 1px solid #e4e4e7;
  padding: 20px 48px;
  min-height: 100vh;
}

.header {
  padding: 20px 0;
  width: 100%;
  color: #000000;
  font-weight: 500;
  align-items: center;
  display: flex;
  box-sizing: border-box;
  justify-content: space-between;
  outline: 1px solid blue;
}


.task {
  width: 280px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  margin: 0;
}

</style>