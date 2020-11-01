<template>
  <div id="app">
    <div id="app">
      <div class="container mt-5">
        <div class="row">
          <div class="col-12">
            <h2>Next Classes</h2>
            <hr />
            <div class="card-columns">
              <nextClass
                v-for="classData in state.nextClassesData"
                :key="classData.id"
                :classData="classData"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { store } from "./store";
import NextClass from "./components/NextClass";

export default {
  name: "App",
  data() {
    console.log(store);
    return {
      state: store.state,
    };
  },
  mounted() {
    this.$http
      .post(
        "https://www.sportsnow.ch/platform/api/v1/public/provider/leone-academy-liebefeld/live_calendar"
      )
      .then((response) => {
        console.log("SPORTSNOW_RESPONSE: ", response.data);
        this.state.nextClassesData = response.data;
      });
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
