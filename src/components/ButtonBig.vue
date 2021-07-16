<template>
  <div class="btn-big">
    <button><span class="material-icons">add</span>{{label}}</button>
  </div>
</template>

<script>
export default {
  name: "ButtonBig",
  props: ['label']
}
</script>

<style scoped lang="scss">
.btn-big {
  height: 64px;
  display: flex;
  width: 100%;
  button {
    color: #868f9c;
    border: none;
    font-size: 16px;
    font-family: 'Roboto', sans-serif;
    cursor: pointer;
    width: 100%;
    border-radius: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgb(238, 238, 238);
    transition: 0.2s background-color;
    &:hover {
      background: rgba(238, 238, 238, 0.8);
    }
    span {
      margin-right: 8px;
    }
  }
}
</style>