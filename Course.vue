<template>
  <div v-if="lecture.data" class="lectures">
    <ViewLecture :lecture="lecture" :isCourse=true />
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
    mode: "out-in"
  },
  async asyncData({ $axios, params, redirect, error }) {
    const lecture = await $axios
      .$get(
        `/course/${encodeURIComponent(
          params.course
        )}/lesson/${encodeURIComponent(params.lecture)}`
      )
      .catch(invalid => {
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
        .post(`/course/${data.course.slug}/lesson/${data.lesson.slug}/complete`)
        .then(() => {
          let route = `/courses/${this.$route.params.course}`;
          if (this.lecture.data.course.hasOwnProperty("next_item")) {
            route += `/${data.course.next_item.slug}`;
          }
          this.$router.push(route);
        });
    }
  }
};
</script>

<style scope module>
img {
  width: auto;
  max-width: 1100px;
  height: auto;
  object-fit: contain;
  display: block;
}
.btn__primary-lecture {
  color: red;
}
</style>

<style scoped>
[v-cloak] {
    display: none;
}
</style>
