<template>
  <button type="button" class="btn-close">
    <span class="material-icons">close</span>
  </button>
</template>

<script>
export default {
  name: "ButtonClose"
}
</script>

<style scoped lang="scss">
.btn-close {
  border: none;
  background: transparent;
  margin-right: 16px;
  cursor: pointer;

  &:hover span {
    color: #000000;
  }

  span {
    color: #868f9c;
    font-size: 28px;
    transition: 0.2s;
  }
}
</style>