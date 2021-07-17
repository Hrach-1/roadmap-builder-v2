<template>
  <div class="tooltip">
    <ul>
      <li class="bb material-icons" @click="$emit('edit')">edit</li>
      <li class="material-icons" @click="$emit('delete')">delete</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Tooltip"
}
</script>

<style scoped lang="scss">
  .tooltip {
    position: absolute;
    //right: 0;
    //bottom: 0;
    //top: 0px;
    left: 28px;
    background-color: rgba(248, 248, 248, 0.73);
    border-radius: 4px;
    border: 1px solid #a0a0a0;
    ul {
      list-style: none;
      padding: 0;
      margin: 0;
      li {
        cursor:pointer;
        padding: 8px 8px 8px 8px;
        margin: 0;
        border-radius: 0 0 4px 4px ;
        transition: 0.2s background-color;

        &:hover {
          background-color: rgb(234, 234, 234);
        }

        &.bb {
          border-bottom: 1px solid #a0a0a0;
          border-radius: 4px 4px 0 0 ;
        }
      }
    }
  }
</style>