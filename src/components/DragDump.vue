<template>
  <VueDraggable
    :group="{ name: groupName, pull: false, put: true }"
    draggable=".dump-draggable"
    ghostClass="dump-removed"
    ><slot
  /></VueDraggable>
</template>

<script>
import VueDraggable from 'vuedraggable';

export default {
  name: 'DragDump',

  components: {
    VueDraggable,
  },

  props: {
    groupName: {
      type: String,
      default: '',
    },
  },
};
</script>

<style lang="scss">
.dump-removed {
  display: none;
}
</style>
