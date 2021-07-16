<template>
  <div class="add-list-block" :class="{
    first: taskListTitles.length === 0
  }">
    <ButtonBig label="Add a list" @click="toggleAddList" v-show="!showAddList"/>
    <div class="add-task"  v-show="showAddList">
      <TextareaPrimary @keypress.enter="addList" placeholder="Enter list title" ref="addList"/>
      <div class="toolbar">
        <ButtonClose @click="toggleAddList"/>
        <ButtonPrimary label="Add list" @click="addList"/>
      </div>
    </div>
  </div>


</template>

<script>
import ButtonPrimary from "@/components/ButtonPrimary";
import ButtonClose from "@/components/ButtonClose";
import ButtonBig from "@/components/ButtonBig";
import TextareaPrimary from "@/components/TextareaPrimary";

export default {
  name: 'AddList',
  components: {TextareaPrimary, ButtonBig, ButtonClose, ButtonPrimary},
  props: {
    'tasks': {
      type: Array,
      required: true
    },
    'taskListTitles': {
      type: Array,
      required: true
    }
  },
  data: () => ({
    showAddList: false
  }),
  methods: {
    toggleAddList() {
      this.showAddList = !this.showAddList
      if (this.showAddList) {
        this.$refs.addList.focus()
      }
    },
    addList() {
      const $textarea = this.$refs.addList
      if ($textarea.value) {
        this.tasks.push([])
        this.taskListTitles.push($textarea.value)
        $textarea.clear()
        this.toggleAddList()
      }
    },
  }
}
</script>

<style scoped lang="scss">
.add-list-block {
  min-width: 322px;
  box-sizing: border-box;
  margin-left: 48px;

  &.first {
    margin-left: 0;
  }

  .toolbar {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    margin-top: 16px;
  }
}



</style>
