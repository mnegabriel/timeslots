<template>
  <div
    class="courseBlock"
    :class="[
      disabled ? 'courseBlock--disabled' : draggableClass,
      asTimeslot && 'courseBlock--timeslot',
    ]"
    :style="{
      '--block-color': course.color,
      '--block-hours': course.hours,
    }"
  >
    <div
      class="courseBlock__wrapper"
      :class="{ 'courseBlock__wrapper--gradient': gradient }"
      ref="blockWrapper"
    >
      <span class="courseBlock__name" ref="courseName">{{ course.name }}</span>

      <div class="courseBlock__utils" :class="{ disabled }">
        <span class="courseBlock__drag" :class="[handleClass]">&amp;</span>

        <span class="courseBlock__hours">{{ course.hours }} h</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CourseBlock',

  props: {
    course: {
      type: Object,
      default: () => ({}),
    },

    draggableClass: {
      type: String,
      default: '',
    },

    handleClass: {
      type: String,
      default: '',
    },

    disabled: {
      type: Boolean,
      default: false,
    },

    asTimeslot: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      gradient: false,
    };
  },

  mounted() {
    const blockWrapper = this.$refs.blockWrapper.$el
      ? this.$refs.blockWrapper.$el
      : this.$refs.blockWrapper;
    const courseName = this.$refs.courseName.$el
      ? this.$refs.courseName.$el
      : this.$refs.courseName;

    if (
      blockWrapper.clientWidth < courseName.clientWidth
      || courseName.clientHeight < courseName.scrollHeight
    ) {
      this.gradient = true;
    }
  },
};
</script>

<style lang="scss" scoped>
.courseBlock {
  --crsblck-clr: var(--block-color, #363636);
  --crsblck-disabled-clr: var(--block-color-disabled, #bfbfbf);
  //
  --crsblck-fnt-sz: var(--block-font-size, 0.75rem);
  --crsblck-fnt-clr: var(--block-font-color, white);
  --crsblck-ln-hght: var(--block-line-height, 1.25);
  //
  --crsblck-pddng: var(--block-padding, 7px);
  --crsblck-pddng-hrs: var(--block-padding-hours, 10px);
  --crsblck-spcng: var(--block-spacing, 12px);
  --crsblck-rds: var(--block-radius, 4px);
  //
  --crsblck-wdth-tmblck: calc(
    calc(var(--block-hours, 1) / var(--block-hours-total, 1)) * 100%
  );

  background-color: var(--crsblck-clr);
  font-size: var(--crsblck-fnt-sz);
  line-height: var(--crsblck-ln-hght);
  //
  padding: var(--crsblck-pddng);
  border: 1px solid var(--crsblck-clr);
  border-radius: var(--crsblck-rds);
  //
  position: relative;

  &--disabled {
    --crsblck-fnt-clr: var(--crsblck-disabled-clr);

    background-color: transparent;
    border-color: var(--crsblck-disabled-clr);
    border-style: dashed;
  }

  &__wrapper {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  &__name {
    color: var(--crsblck-fnt-clr);
    font-weight: bold;
  }

  &__utils {
    margin-left: auto;
    display: inline-flex;
    align-items: center;
    gap: 10px;

    &.disabled {
      visibility: hidden;
    }
  }

  &__drag {
    display: inline-block;
    color: white;
    opacity: 0.9;
    cursor: grab;

    &:not(:hover) {
      opacity: 0.7;
    }
  }

  &__hours {
    display: inline-block;
    background-color: white;
    border-radius: var(--crsblck-rds);
    padding: var(--crsblck-pddng-hrs);
    //
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }

  &--timeslot {
    @extend %timeslot;
  }

  &--ghost {
    @extend %timeslot;
    opacity: 0.5;
  }
}

%timeslot {
  --crsblck-pddng: var(--block-padding-small, 4px);
  --crsblck-pddng-hrs: var(--block-padding-hours-small, 4px);

  width: var(--crsblck-wdth-tmblck);

  .courseBlock {
    &__wrapper {
      align-items: flex-end;
      gap: 0;
      //
      height: 100%;
      padding-top: calc(
        var(--crsblck-fnt-sz) + calc(2 * var(--crsblck-pddng)) +
          var(--crsblck-spcng)
      );
      //
      overflow: hidden;
      position: relative;

      &--gradient::after {
        content: "";
        position: absolute;
        top: 0;
        right: 0;
        height: 100%;
        width: min(20px, 50%);
        background-image: linear-gradient(
          90deg,
          transparent,
          var(--crsblck-clr)
        );
        opacity: 1;
      }
    }

    &__utils {
      position: absolute;
      z-index: 1;
      top: 0;
      right: 0;
      //
      display: inline-flex;
      gap: 3px;
    }

    &__name {
      max-height: calc(2em * var(--crsblck-ln-hght));
    }
  }
}
</style>
