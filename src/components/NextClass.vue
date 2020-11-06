<template>
  <div class="card">
    <div class="card-header text-center bg-vue">
      <strong> {{ classData.name }} </strong>
    </div>
    <div class="card-body">
      <strong>{{ dayName }}</strong>
      <div>{{ classData.time_begin }} - {{ classData.time_end }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "NextClass",
  props: ["classData"],
  components: {},
  computed: {
    dayName: (classData) => {
      let d = new Date(classData.classData.date);
      return d.toLocaleString(window.navigator.language, { weekday: "long" });
    },
  },
};
</script>

<style></style>
