<template>
  <div class="card">
    <div class="card-header text-center bg-vue">
      <strong> {{ classData.name }} </strong>
    </div>
    <div class="card-body">
      <div>{{ classData.time_begin }} - {{ classData.time_end }}</div>
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
