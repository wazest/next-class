<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row">
        <div class="col-16">
          <h5>Aktuelles Training</h5>
          <hr />
          <div class="card-columns">
            <div class="info" v-if="!currentClass">
              Zur Zeit läuft kein Training.
            </div>
            <nextClass v-if="currentClass" :classData="currentClass" />
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-16">
          <h5>Kommende Trainings</h5>
          <hr />
          <div class="card-deck">
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
      classesSortedbyDateTime: null,
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
      this.classesSortedbyDateTime = this.classesSortedbyDate.sort((x, y) => {
        return x.time_begin - y.time_begin;
      });

      this.classesSortedbyDateTime.find((cl) => {
        if (cl.date >= this.getQueryDate()) {
          if (
            cl.time_begin > this.getQueryTime() &&
            cl.date == this.getQueryDate() &&
            this.nextThreeClasses.length <= 2
          ) {
            // coming classes this day
            this.nextThreeClasses.push(cl);
          }
          if (
            cl.date > this.getQueryDate() &&
            this.nextThreeClasses.length <= 2
          ) {
            // coming classes next day
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
    },
    getQueryDate() {
      let today = new Date();
      let dd = String(today.getDate()).padStart(2, "0");
      let mm = String(today.getMonth() + 1).padStart(2, "0");
      let yyyy = today.getFullYear();
      let queryDate = yyyy + "-" + mm + "-" + dd;
      return queryDate;
      // return "2020-11-05";
    },
    getQueryTime() {
      let today = new Date();
      let hh = String(today.getHours()).padStart(2, "0");
      let mm = String(today.getMinutes()).padStart(2, "0");

      let time = hh + ":" + mm;
      return time;
      // return "19:01";
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
.info {
  margin: 60px;
  float: left;
}
</style>
