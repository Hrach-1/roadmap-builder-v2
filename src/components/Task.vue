<template>

    <div
      class="task-box"
      draggable="true"
      @dragstart="dragStart"
      @dragend="dragEnd"
      :class="{
        first: idx === 0
      }"
      v-show="!edit"
      :data-task-list="list"
    >
      <p class="task" :data-task-list="list" draggable="false">{{ task }}</p>
      <div class="options" draggable="false" :data-task-list="list">
        <ButtonMore draggable="false" :data-task-list="list" @click="tooltipShow = !tooltipShow"/>
        <Tooltip @edit="toggleEdit" @delete="deleteTask" v-if="tooltipShow" :data-task-list="list"/>
        <div class="avatar-thumbnail" draggable="false" :data-task-list="list">
          <img src="../assets/avatar.svg" alt="user" width="24" draggable="false" :data-task-list="list">
        </div>
      </div>
    </div>

    <div class="edit-block" v-show="edit" :data-task-list="list">
      <TextareaPrimary  ref="editArea" @keypress.enter="saveEdit" @keydown.esc="toggleEdit" :data-task-list="list"/>
      <div class="toolbar" :data-task-list="list">
        <ButtonClose @click="toggleEdit" :data-task-list="list"/>
        <ButtonPrimary label="save" @click="saveEdit" :data-task-list="list"/>
      </div>
    </div>
</template>

<script>
import ButtonPrimary from "@/components/ButtonPrimary";

import Tooltip from "@/components/Tooltip";
import ButtonMore from "@/components/ButtonMore";
import TextareaPrimary from "@/components/TextareaPrimary";
import ButtonClose from "@/components/ButtonClose";

export default {
  name: 'Task',
  components: {ButtonClose, ButtonPrimary, TextareaPrimary, ButtonMore, Tooltip},
  data: () => ({
    edit: false,
    tooltipShow:false,
    editAreaRows: 1,
    hideTask: true
  }),
  props: {
    task: {
      require: true,
      type: String,
    },
    list: Number,
    idx: Number,
  },
  mounted() {
    this.editAreaRows = Math.ceil((this.task.length)/29) || 1
  },
  inject: ['nowDrag'],
  methods: {
    dragStart(e) {
      document.querySelectorAll('.dropzone').forEach((el) => {
        el.style.display = 'none'
      })
      e.target.style.opacity = 0.4
      this.nowDrag.list = this.list
      this.nowDrag.idx = this.idx
    },
    dragEnd(e) {
      e.target.style.opacity = 1
    },
    toggleEdit() {
      this.edit = !this.edit
      console.log(this.edit)
      if (this.edit) {
          const $textarea =this.$refs.editArea
          $textarea.focus()
          $textarea.text = this.task
      }
    },
    saveEdit() {
      this.$emit('editTask', {text: this.$refs.editArea.text, list: this.list, idx: this.idx})
      this.edit = !this.edit
      this.tooltipShow = false
    },
    deleteTask() {
      this.$emit('deleteTask', {list: this.list, idx: this.idx})
      this.tooltipShow = false
    },
  },
}
</script>

<style scoped lang="scss">
.edit-block {
  .toolbar {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    margin-top: 16px;
  }
}



.task-box {
  border: 1px solid #e4e4e4;
  padding: 5px 16px;
  border-radius: 4px;
  width: 322px;
  color: #868f9c;
  font-weight: normal;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: grab;
  box-shadow: 0.5px 2px 14px 0px #e4e4e4;
  transition: 0.2s;
  box-sizing: border-box;
  background: #ffffff;
  margin-top: 24px;

  &:hover {
    box-shadow: 6px 10px 11px 0 #e4e4e4;
  }

  &.first {
    margin-top: 0;
  }

  .task {
    width: 260px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    margin: 0;
  }

  .options {
    transition: 0.2s;
    display: flex;
    flex-direction: column;
    position: relative;

    .avatar-thumbnail {
      margin: 0;
    }
  }
}
</style>
