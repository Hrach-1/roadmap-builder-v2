<template>
  <div class="roadmap" ref="roadmap">
    <TaskList title="Planned" :now-drag="nowDrag" :tasks="tasks" :first-list="true" :task-list="0"/>
    <TaskList title="Under consideration" :now-drag="nowDrag" :tasks="tasks" :task-list="1"/>
    <TaskList title="Already Shipped" :now-drag="nowDrag" :tasks="tasks" :task-list="2"/>
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
    nowDrag: {}
  }),
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
</style>
