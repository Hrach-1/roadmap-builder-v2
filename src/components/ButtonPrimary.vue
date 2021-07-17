<template>
  <button type="button" class="btn">{{label}}</button>
</template>

<script>
export default {
  name: "ButtonPrimary",
  props: ['label']
}
</script>

<style scoped lang="scss">
.btn {
  border: 1px solid #e4e4e7;
  border-radius: 8px;
  background: transparent;
  padding: 8px 8px;
  color: #868f9c;
  cursor: pointer;
  transition: 0.2s;

&:hover {
   background: #f3f3f3;
 }
}
</style>