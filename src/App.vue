<!-- :now-drag="nowDrag" -->

<template>
  <div class="roadmap" ref="roadmap">
    <TaskList
      v-for="(title, idx) in taskListTitles"
      :key="idx"
      :title="title"
      :task-list="idx"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Task from '@/components/Task'
import TaskList from '@/components/TaskList'
import AddTask from '@/components/AddTask'

export default {
  name: 'App',
  components: {
    AddTask,
    TaskList,
    Task,
  },
  data: () => ({
    taskListTitles: ['Planned', 'Under consideration', 'Already Shipped'],
    tasks: [
      [
        'Faster refresh rate on event plan',
        'Instagram stories',
        'Image optimization, minify JS/CSS/HTML',
        'Facebook groups source',
        'Shopify App',
      ],
      [
        'Web hooks for article deletion',
        'Facebook Groups Source',
        'Shopify App',
      ],
      [
        'Small resolution images',
        'Tiktok source',
        'Image optimization, minify JS/CSS/HTML',
        'Turn disconnection emails off fortnite',
        'Instagram @mention source',
        'More detailed help videos',
      ],
    ],
  }),
  provide: {
    nowDrag: {},
  },
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
