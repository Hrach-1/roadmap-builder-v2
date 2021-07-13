<template>
  <div class="task-box">
    <p class="task">{{task}}</p>
    <div class="options">
      <img src="../assets/more.svg" alt="more" width="16">
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
    }
  },
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
}

.task-box:hover {
  box-shadow: 1px 8px 14px 0px #e4e4e4;
}

.task {
  width: 260px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  margin: 0;
}

</style>