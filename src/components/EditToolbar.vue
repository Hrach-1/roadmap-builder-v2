<template>
  <div class="toolbar">
    <ButtonClose @click="$emit('toolbarClose')"/>
    <ButtonPrimary label="save"  @click="$emit('toolbarSave')"/>
  </div>
</template>

<script>
import ButtonClose from "@/components/ButtonClose";
import ButtonPrimary from "@/components/ButtonPrimary";
export default {
  name: "EditToolbar",
  components: {ButtonPrimary, ButtonClose}
}
</script>

<style scoped>
.toolbar {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin-top: 16px;
}
</style>