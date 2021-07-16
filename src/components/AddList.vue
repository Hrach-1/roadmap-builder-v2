<template>
  <div class="add-list-block">
   <ButtonBig label="Add a list" @click="toggleAddList"/>
      <div class="add-task" @keypress.enter="addList">
<!--        <textarea v-model="task" ref="task" rows="1" cols="30" placeholder="Enter a note"></textarea>-->
        <TextareaPrimary placeholder="Enter list title"/>
        <div class="toolbar">
          <ButtonClose/>
          <ButtonPrimary label="Add list"/>
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
  props: ['tasksList', 'list', 'addTaskState'],
  data: () => ({
    task: '',
  }),
  methods: {
    toggleAddList() {

    },
    addTask(e) {
      if (this.task) this.tasksList.unshift(this.task)
      this.task = ''
      setTimeout(() => this.$refs.task.rows = 1, 10)

      this.$emit('hide', this.list)
    },
    focus() {
      setTimeout(() => this.$refs.task.focus(), 10)
    },
    close() {
      this.$emit('hide', this.list)
    }
  },
  watch: {
    task(val, oldVal) {
      if (val.length > 1) {
        if ((val.length - 1) % 29 === 0 && val.length > oldVal.length) this.$refs.task.rows = +this.$refs.task.rows + 1
        else if ((val.length) % 29 === 0 && val.length < oldVal.length) this.$refs.task.rows = +this.$refs.task.rows - 1
      }
    }
  }
}
</script>

<style scoped lang="scss">
.add-list-block {
  width: 382px;
}

.add-task {
  overflow: auto;
  margin-bottom: 24px;

  textarea {
    font-size: 18px;
    border: 1px solid #e4e4e7;
    border-radius: 4px;
    color: #868f9c;
    font-weight: normal;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    box-shadow: 0.5px 2px 14px 0px #e4e4e4;
    transition: 0.2s;
    box-sizing: border-box;
    resize: none;
    padding: 20px 16px;
    font-family: 'Roboto', sans-serif;

    &:focus {
      outline: none;
    }
  }

  .toolbar {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    margin-top: 16px;

  }
}
</style>
