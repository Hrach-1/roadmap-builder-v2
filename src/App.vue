<template>
  <div id="roadmap" ref="roadmap" :class="roadmapHeight">
    <TaskList
      v-for="(title, idx) in taskListTitles"
      :key="idx"
      :titles="taskListTitles"
      :task-list="idx"
      :tasks="tasks"
    />
    <AddList :tasks="tasks" :task-list-titles="taskListTitles"/>
  </div>
</template>

<script>
import Task from '@/components/Task'
import TaskList from '@/components/TaskList'
import AddTask from '@/components/AddTask'
import AddList from "@/components/AddList";

export default {
  name: 'App',
  components: {
    AddList,
    AddTask,
    TaskList,
    Task,
  },

  data: () => ({
    ClientHeight: 0,
    scrollHeight: 18,
    taskListTitles: ['Planned', 'Under consideration', 'Already Shipped'],
    tasks: [
      [
        'Faster refresh rate on event plan',
        'Instagram stories',
        'Image optimization, minify JS/CSS/HTML',
        'Facebook groups source',
        'Shopify App'
      ],
      [
        'Web hook for article deletion',
        'Facebook Groups Source',
        'Shopify App 2'
      ],
      [
        'Small resolution images',
        'Tiktok source',
        'Image optimization, minify JS/CSS/HTML 2',
        'Turn disconnection emails off for something',
        'Instagram @mention source',
        'More detailed help videos'
      ]
    ],
  }),
  computed: {
    roadmapHeight() {
      return {
        minHeight: `calc(100vh - 40px - ${this.scrollHeight}px}`
      }
    }
  },
  mounted() {
    this.ClientHeight = document. documentElement.clientHeight
  },
  updated() {
    this.ClientHeight = document. documentElement.clientHeight
  },
  watch: {
    ClientHeight(val, oldVal) {
      if (val > oldVal) {
        this.scrollHeight = 0
      }else {
        this.scrollHeight = 18
      }
    }
  },
  provide: {
    nowDrag: {},
  },
}
</script>

<style lang="scss">
body {
  font-family: 'Roboto', sans-serif;
  margin: 0;
}

#roadmap {
  margin: 0;
  display: flex;
  padding: 20px 32px;
  min-height: calc(100vh - 40px - 24px);
  width: fit-content;
}

.material-icons {
  font-family: 'Material Icons';
  font-weight: normal;
  font-style: normal;
  font-size: 24px; /* Preferred icon size */
  display: inline-block;
  line-height: 1;
  text-transform: none;
  letter-spacing: normal;
  word-wrap: normal;
  white-space: nowrap;
  direction: ltr;

  /* Support for all WebKit browsers. */
  -webkit-font-smoothing: antialiased;
  /* Support for Safari and Chrome. */
  text-rendering: optimizeLegibility;

  /* Support for Firefox. */
  -moz-osx-font-smoothing: grayscale;

  /* Support for IE. */
  font-feature-settings: 'liga';
}
</style>
