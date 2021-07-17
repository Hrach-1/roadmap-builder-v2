<template>
  <button class="btn-more">
        <span class="material-icons">more_horiz</span>
  </button>
</template>

<script>
export default {
  name: "ButtonMore"
}
</script>

<style scoped lang="scss">
.btn-more {
  border: none;
  padding:0;
  margin: 0;
  background: transparent;
  cursor: pointer;

  &:hover span {
    color: #000000;
  }

  span {
    color: #868f9c;
    transition: 0.2s;
  }
}

</style>