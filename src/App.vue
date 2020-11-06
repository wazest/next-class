<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row">
        <div class="col-12">
          <h5>Aktuelles Training</h5>
          <hr />
          <div class="card-columns">
            <div v-if="!currentClass">Zur Zeit läuft kein Training</div>
            <nextClass v-if="currentClass" :classData="currentClass" />
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-12">
          <h5>Kommende Trainings</h5>
          <hr />
          <div class="card-columns">
            <nextClass
              v-for="classData in nextThreeClasses"
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
      classesSortedbyDate: null,
      nextThreeClasses: [],
      currentClass: null,
      loading: true,
      errored: false,
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async processData(response) {
      this.classesSortedbyDate = response.sort((x, y) => {
        return x.date - y.date;
      });
      response.find((cl) => {
        if (cl.date >= this.getQueryDate()) {
          if (
            cl.time_begin > this.getQueryTime() &&
            this.nextThreeClasses.length <= 2
          ) {
            this.nextThreeClasses.push(cl);
          }
          if (
            cl.date === this.getQueryDate() &&
            cl.time_begin < this.getQueryTime() &&
            cl.time_end > this.getQueryTime()
          ) {
            this.currentClass = cl;
          }
        }
      });
      console.log("CLASSES FOUND: ", this.nextThreeClasses);
    },
    getQueryDate() {
      let today = new Date();
      let dd = String(today.getDate()).padStart(2, "0");
      let mm = String(today.getMonth() + 1).padStart(2, "0");
      let yyyy = today.getFullYear();
      console.log("TODAY IS: ", today.getDay());
      let queryDate = yyyy + "-" + mm + "-" + dd;
      console.log("WE QUERY: ", queryDate);
      return queryDate;
      // return "2020-11-05";
    },
    getQueryTime() {
      let today = new Date();
      let hh = String(today.getHours()).padStart(2, "0");
      let mm = String(today.getMinutes()).padStart(2, "0");

      let time = hh + ":" + mm;
      console.log("QUERY TIME: ", time);
      return time;
    },
    async fetchData() {
      this.$http
        .post(
          "https://www.sportsnow.ch/platform/api/v1/public/provider/leone-academy-liebefeld/live_calendar",
          {
            date: this.getQueryDate(),
          }
        )
        .then((response) => {
          console.log("SPORTSNOW_RESPONSE: ", response.data);
          if (response.data && response.data.length) {
            this.nextThreeClasses = [];
            this.currentClass = null;
            this.processData(response.data);
          }
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => {
          this.loading = false;
          setTimeout(() => this.fetchData(), 2 * 60 * 1000); // 2min
        });
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
