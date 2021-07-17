<template>
  <div class="tooltip">
    <ul>
      <li class="material-icons" @click="$emit('add')" v-if="add">add</li>
      <li class="material-icons" @click="$emit('edit')" v-if="edit">edit</li>
      <li class="material-icons" @click="$emit('delete')" v-if="del" >delete</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Tooltip",
  props: {
    edit: {
      type: Boolean,
      default: true
    },
    del: {
      type: Boolean,
      default: true
    },
    add: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang="scss">
  .tooltip {
    position: absolute;
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
        transition: 0.2s background-color;
        color: #868f9c;
        &:hover {
          background-color: rgb(234, 234, 234);
          color: #000000;
        }

        &:first-child {
          border-radius: 4px 4px 0 0 ;

        }
        &:not(:last-child) {
          border-bottom: 1px solid #a0a0a0;
        }
        &:last-child {
          border-radius: 0 0 4px 4px ;
        }
      }
    }
  }
</style>