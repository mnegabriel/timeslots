<template>
  <VueDraggable
    v-model="model"
    :handle="handleSelector"
    :group="{ name: groupName, pull: true, put: ableToRecieve }"
    :ghost-class="ghostClass"
    @start="$emit('start', $event)"
    @end="$emit('end', $event)"
  >
    <slot />
  </VueDraggable>
</template>

<script>
import VueDraggable from 'vuedraggable';

export default {
  name: 'DragReceiver',

  components: {
    VueDraggable,
  },

  props: {
    groupName: {
      type: String,
      default: '',
    },

    ableToRecieve: {
      type: Boolean,
      default: true,
    },

    value: {
      type: Array,
      default: () => [],
    },

    handleSelector: {
      type: String,
      default: '',
    },

    ghostClass: {
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
  },
};
</script>
