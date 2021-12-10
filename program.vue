<template>
  <div v-if="lecture.data" class="lectures">
    <ViewLecture :lecture="lecture" />
  </div>
</template>

<script>
import ViewLecture from "~/components/programs/layouts/ViewLecture";
export default {
  name: "index",
  components: { ViewLecture },
  layout: "educationProgram",
  transition: {
    name: "programs",
    mode: "out-in",
  },
  async asyncData({ $axios, params, redirect, error }) {
    const lecture = await $axios
      .$get(
        `/programs/${params.program_id}/course/${params.course_id}/lesson/${params.lecture_id}`
      )
      .catch((invalid) => {
        if (invalid.response.status === 401) {
          redirect({ name: "login" });
        } else if (invalid.response.status === 404) {
          error({ statusCode: 404, message: "Лекция не найдена" });
        }
      });
    return { lecture };
  },
  methods: {
    finishLesson() {
      const data = this.lecture.data;
      this.$axios
        .post(
          `/programs/${data.education_program.id}/course/${data.course.id}/lesson/${data.lesson.id}/complete`
        )
        .then(
          this.$router.push(
            `/programs/${this.$route.params.program_id}/${this.$route.params.course_id}`
          )
        );
    },
  },
};
</script>

<style scope module>
</style>
