<template>
  <div class="home">
    <DragDump group-name="courses">
      <div class="home__banner">
        <h1>Banner Simples</h1>
      </div>

      <div class="home__content">
        <div class="home__contexts">
          <template v-for="context of contexts">
            <div class="home__context" :key="context._key">
              <h5 class="home__contextName">{{ context.name }}</h5>

              <div class="home__sections">
                <template v-for="(section, idx) of context.sections">
                  <CourseSection :key="section._key" :section="section">
                    <template #courses="{ list }">
                      <DragProvider
                        draggable-selector=".course-unselected"
                        handle-selector=".course-grab-handle"
                        group-name="courses"
                        v-model="context.sections[idx].courses"
                        @start="handleStart"
                        @end="handleEnd"
                      >
                        <template v-for="course of list">
                          <CourseBlock
                            :key="course._key"
                            :course="course"
                            :data-key="course._key"
                            :disabled="selectedCourses.includes(course._key)"
                            handle-class="course-grab-handle"
                            draggable-class="course-unselected"
                          />
                        </template>
                      </DragProvider>
                    </template>
                  </CourseSection>
                </template>
              </div>
            </div>
          </template>
        </div>

        <div class="home__semesters">
          <button @click="addNewSemester">Add Semester</button>

          <template v-for="(sem, i) of semesters">
            <DragReceiver
              class="home__semester"
              group-name="courses"
              handle-selector=".timeblock-draggable"
              ghost-class="courseBlock--ghost"
              v-model="semesters[i].courses"
              :key="i"
              :ableToRecieve="!semestersUnfit[i]"
              :style="{ '--block-hours-total': sem.maxHours }"
              @start="handleStart"
              @end="handleEnd"
            >
              <template v-for="course of semesters[i].courses">
                <CourseBlock
                  handle-class="timeblock-draggable"
                  :key="course._key"
                  :data-key="course._key"
                  :course="course"
                  :blockWidth="course.hours * 4"
                  as-timeslot
                />
              </template>
            </DragReceiver>

            <p :key="i + '-details'">
              Horas: {{ currentHoursAmount[i] }}/{{ sem.maxHours }}
            </p>
          </template>
        </div>
      </div>
    </DragDump>
  </div>
</template>

<script>
import contexts from '@/data/contexts';

import DragDump from '../components/DragDump.vue';
import DragProvider from '../components/DragProvider.vue';
import DragReceiver from '../components/DragReceiver.vue';
import CourseSection from '../components/CourseSection.vue';
import CourseBlock from '../components/CourseBlock.vue';

export default {
  name: 'Home',

  components: {
    DragDump,
    DragReceiver,
    DragProvider,
    CourseSection,
    CourseBlock,
  },

  data() {
    return {
      contexts,
      baseSemester: {
        courses: [],
        maxHours: 50,
      },
      semesters: [],
      courseOnHold: null,
    };
  },

  computed: {
    selectedCourses() {
      const selectedSet = new Set(
        this.semesters
          .map((sem) => sem.courses)
          .flat()
          // eslint-disable-next-line no-underscore-dangle
          .map((c) => c._key),
      );
      return [...selectedSet];
    },

    currentHoursAmount() {
      return this.semesters.map((sem) => sem.courses.reduce((acc, cour) => acc + cour.hours, 0));
    },

    semestersUnfit() {
      return this.semesters.map((sem, i) => {
        if (this.courseOnHold === null || this.courseOnHold === undefined) {
          return false;
        }
        return (
          !sem.courses.includes(this.courseOnHold)
          && this.currentHoursAmount[i] + this.courseOnHold.hours > sem.maxHours
        );
      });
    },

    onlyCoursesFromContexts() {
      return this.contexts
        .map((ctx) => ctx.sections.map((sec) => sec.courses).flat())
        .flat();
    },
  },

  methods: {
    addNewSemester() {
      this.semesters.push(JSON.parse(JSON.stringify(this.baseSemester)));
    },

    handleStart(event) {
      this.courseOnHold = this.onlyCoursesFromContexts
        // eslint-disable-next-line no-underscore-dangle
        .find((cour) => cour._key === event.item.dataset.key);
    },

    handleEnd() {
      this.courseOnHold = null;
    },
  },
};
</script>

<style lang="scss" scoped>
.home {
  &__banner {
    height: 200px;
    background-color: darkcyan;
    display: grid;
    place-items: center;
    margin-bottom: 20px;

    h1 {
      color: white;
    }
  }

  &__content {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 20px;
  }

  &__contexts {
    padding-inline: 10px;
  }

  &__contextName {
    margin-bottom: 13px;
  }

  &__sections {
    display: grid;
    gap: 20px;
  }

  &__semesters {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 20px;
  }

  &__semester {
    background-color: rgb(216, 216, 216);
    width: 100%;
    margin-bottom: 10px;
    min-height: 30px;
    display: flex;
    gap: 4px;
  }
}
</style>
