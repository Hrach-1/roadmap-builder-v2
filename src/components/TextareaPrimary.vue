<template>
  <textarea
    v-model="task"
    ref="task"
    :rows="rows"
    :cols="cols"
    :placeholder="placeholder"
  ></textarea>
</template>

<script>
export default {
  name: "TextareaPrimary",
  props: {
    placeholder: {
      default: '',
      type: String
    },
    Pcols: {
      type: Number,
      default: 30
    },
    Prows: {
      type: Number,
      default: 1
    }
  },
  data: () => ({
    task: '',
    cols: '',
    rows: '',
  }),
  mounted() {
    this.cols = this.Pcols
    this.rows = this.Prows
  },
  methods: {
    focus() {
      setTimeout(() => this.$refs.task.focus(), 10)
    },
  },
  watch: {
    task(val, oldVal) {
      if (val.length > 1) {
        if ((val.length - 1) % (this.cols - 1) === 0 && val.length > oldVal.length) ++this.rows
        else if ((val.length) % (this.cols - 1) === 0 && val.length < oldVal.length) --this.rows
      }
    }
  }
}
</script>

<style scoped lang="scss">
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
    //border: black;
  }
}
</style>