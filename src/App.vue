<template>
  <v-app>
    <v-app-bar app color="#7C0A02" dark>
      <v-row no-gutters>
        <v-col>
          <p>Propstack</p>
        </v-col>
        <v-spacer />
        <v-col>
          <h2 class="h2">SpaceX Launches</h2>
        </v-col>
        <v-spacer />
        <v-col>
          <v-text-field
            placeholder="Search for mission"
            hide-details
            prepend-icon="mdi-magnify"
            single-line
            v-model="searchText"
          ></v-text-field>
        </v-col>
      </v-row>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-timeline v-if="sortedLaunches.length > 0">
          <rocket-timeline
            v-for="launch in sortedLaunches"
            :key="launch.flight_number"
            :launch="launch"
            :num="launch.flight_number"
          />
        </v-timeline>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
import RocketTimeline from "./components/RocketTimeline.vue";
export default {
  name: "App",
  components: {
    RocketTimeline,
  },
  data: () => ({
    launches: [],
    sortedLaunches: [],
    searchText: "",
  }),
  async created() {
    const { data } = await axios.get(
      "https://api.spacexdata.com/v3/launches/past"
    );

    data.forEach((launch) => {
      this.launches.push(launch);
      this.sortedLaunches.push(launch);
    });
  },
  methods: {
    filterLaunches() {
      this.sortedLaunches = [];
      this.launches.forEach((x) => {
        if (x.mission_name.includes(this.searchText)) {
          this.sortedLaunches.push(x);
        }
      });
      if (!this.sortedLaunches.length) {
        this.sortedLaunches = this.launches;
      }
    },
  },
  watch: {
    searchText() {
      this.filterLaunches();
    },
  },
};
</script>

<style>
</style>