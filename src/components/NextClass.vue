<template>
  <div class="card">
    <img class="card-img-top" alt="Card image cap" :src="classImg" />
    <div class="card-body">
      <h5 class="card-title">{{ classData.name }}</h5>
      <div class="card-text">
        <strong>{{ dayName }}</strong>
        <div>{{ classData.time_begin }} - {{ classData.time_end }}</div>
      </div>
    </div>
    <div class="card-footer">
      <small class="text-muted">LEONE ACADEMY</small>
    </div>
  </div>
</template>

<script>
export default {
  name: "NextClass",
  props: ["classData"],
  computed: {
    dayName: (cl) => {
      let d = new Date(cl.classData.date);
      return d.toLocaleString(window.navigator.language, { weekday: "long" });
    },
    classImg: (cl) => {
      if (cl.classData.name && cl.classData.name.includes("Fortgeschritten")) {
        return require("../assets/img/kb-fort.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("Individuell")) {
        return require("../assets/img/kb-indi.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("Anfänger")) {
        return require("../assets/img/kb-anfaenger.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("Fitboxen")) {
        return require("../assets/img/fitboxen.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("fighters")) {
        return require("../assets/img/fighters-only.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("Technik")) {
        return require("../assets/img/kb-technik.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("Sparring")) {
        return require("../assets/img/kb-sparring.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("Athletik")) {
        return require("../assets/img/athletik.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("Kinder")) {
        return require("../assets/img/kids.jpg");
      }
      if (cl.classData.name && cl.classData.name.includes("Yoga")) {
        return require("../assets/img/yoga.jpg");
      }
    },
  },
};
</script>

<style></style>
