<template>
  <VueDraggable
    v-model="model"
    v-bind="draggableOptions"
    @start="$emit('start', $event)"
    @end="$emit('end', $event)"
  >
    <slot />
  </VueDraggable>
</template>

<script>
import VueDraggable from 'vuedraggable';

export default {
  name: 'DragProvider',

  components: {
    VueDraggable,
  },

  props: {
    groupName: {
      type: String,
      default: '',
    },

    value: {
      type: Array,
      default: () => [],
    },

    draggableSelector: {
      type: [String],
      default: '',
    },

    handleSelector: {
      type: String,
      default: '',
    },
  },

  computed: {
    model: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit('input', value);
      },
    },

    draggableOptions() {
      const localOptions = {
        class: 'drag-provider',
        sort: false,
        group: { name: this.groupName, pull: 'clone', put: false },
      };

      if (this.draggableSelector) { localOptions.draggable = this.draggableSelector; }
      if (this.handleSelector) { localOptions.handle = this.handleSelector; }

      return localOptions;
    },
  },
};
</script>

<style lang="scss" scoped>
.drag-provider {
  --drgprvdr-gap: var(--gap, 10px);

  display: grid;
  gap: var(--drgprvdr-gap);
}
</style>
