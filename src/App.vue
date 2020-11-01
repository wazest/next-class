<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row">
        <div class="col-12">
          <h2>Aktuelles Training</h2>
          <hr />
          <div class="card-columns">
            <div v-if="!currentClass">Zur Zeit läuft kein Training</div>
            <!-- <nextClass
                v-for="classData in nextClassesSorted"
                :key="classData.id"
                :classData="classData"
              /> -->
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-12">
          <h2>Kommende Trainings</h2>
          <hr />
          <div class="card-columns">
            <nextClass
              v-for="classData in classesSortedbyDate"
              :key="classData.id"
              :classData="classData"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NextClass from "./components/NextClass";

export default {
  name: "App",
  data() {
    return {
      classesSortedbyDate: "",
      nextThreeClasses: "",
      currentClass: "",
    };
  },
  mounted() {
    // TODO: get current date and query data with it
    this.$http
      .post(
        "https://www.sportsnow.ch/platform/api/v1/public/provider/leone-academy-liebefeld/live_calendar",
        {
          date: "2020-11-02", // 01
        }
      )
      .then((response) => {
        console.log("SPORTSNOW_RESPONSE: ", response.data);
        response.data &&
          response.data.length &&
          this.processData(response.data);
      });
  },
  methods: {
    async processData(response) {
      this.classesSortedbyDate = response
        .sort((x, y) => {
          return x.date - y.date;
        })
        .slice(0, 3);
    },
  },
  components: {
    NextClass,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
