<template>
  <textarea
    v-model="text"
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
    text: '',
    cols: '',
    rows: '',
    oldClientWidth: 0
  }),
  mounted() {
    this.cols = this.Pcols
    this.rows = this.Prows
    this.oldClientWidth = this.$refs.task.clientWidth
  },
  methods: {
    focus() {
      setTimeout(() => this.$refs.task.focus(), 10)
    },
    clear() {
      this.text = ''
      this.rows = 1
    }
  },
  watch: {
    text(val, oldVal) {
      if (val.length > 1) {
        if ((val.length - 1) % (this.cols - 1) === 0 && val.length > oldVal.length) ++this.rows
        else if ((val.length) % (this.cols - 1) === 0 && val.length < oldVal.length) --this.rows
      }
      // if (val.length > 1 && this.$refs.task.clientWidth < this.oldClientWidth) {
      //   if (val.length > oldVal.length) ++this.rows
      //   else if ((val.length) % (this.cols - 1) === 0 && val.length < oldVal.length) --this.rows
      // }
      // if (this.$refs.task.clientWidth < this.oldClientWidth) ++this.rows
      // this.oldClientWidth = this.$refs.task.clientWidth
      // setTimeout(() => {
      //
      // }, 10)
      // console.log(this.text)

      // const numberOfLineBreaks = (val.match(/\n/g)||[]).length;
      // const characterCount = val.length + numberOfLineBreaks;
      // console.log(numberOfLineBreaks)
    }
  }
}
</script>

<style scoped lang="scss">
textarea {
  font-size: 16px;
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
  white-space: pre-wrap;
  width: 100%;
  overflow: hidden;
  overflow-wrap: break-word;
  &:focus {
    outline: none;
  }
}
</style>