<template>
  <div class="courseSection">
    <h6 class="courseSection__title">{{ section.name }}</h6>
    <div class="courseSection__courses">
      <slot name="courses" :list="section.courses" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'CourseSection',

  props: {
    section: {
      type: Object,
      default: () => ({}),
    },
  },
};
</script>

<style lang="scss" scoped>
.courseSection {
  --sctn-pddng: var(--section-padding, 24px);
  --sctn-rds: var(--section-radius, 4px);
  --sctn-grid-gap: var(--section-gap, 20px);

  &__title {
    color: #7c7c7c;
    font-weight: 400;
    font-size: 0.875rem;
  }

  &__courses {
    display: grid;
    gap: var(--sctn-grid-gap);
    border-radius: var(--sctn-rds);
    border: 1px solid #bfbfbf;
    padding: var(--sctn-pddng);
  }
}
</style>
