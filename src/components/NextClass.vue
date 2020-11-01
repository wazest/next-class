<template>
  <div class="card">
    <div class="card-header text-center bg-vue">
      <strong> {{ classData.fullName }} </strong>
    </div>
    <div class="card-body">
      <div>{{ classData.startTime }} - {{ classData.endTime }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "NextClass",
  props: ["classData"],
  components: {},
};
</script>

<style></style>
