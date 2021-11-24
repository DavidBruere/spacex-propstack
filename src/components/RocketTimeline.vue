<template>
  <v-timeline-item :color="changeColor">
    <template v-slot:opposite>
      <span
        ><h3 class="font-weight-bold;">
          Mission: {{ launch.mission_name }}
        </h3></span
      >
    </template>
    <v-card
      v-if="!details"
      @click="showDetails"
      class="elevation-2"
      :style="
        num % 2 ? 'border: 1px solid #7C0A02;' : 'border: 1px solid #F1BC31;'
      "
    >
      <v-card-title class="card headline justify-center">
        Rocket: {{ launch.rocket.rocket_name }}
      </v-card-title>

      <v-img height="200" :src="launch.links.mission_patch_small"></v-img>
      <v-card-text>
        {{ launch.details }}
      </v-card-text>
    </v-card>
    <v-card
      v-if="details"
      @click="showDetails"
      class="elevation-2"
      :style="
        num % 2 ? 'border: 1px solid #7C0A02;' : 'border: 1px solid #F1BC31;'
      "
    >
      <v-card-title class="card headline justify-center">
        {{ launch.rocket.rocket_name + " " + successfulLaunch() }}
      </v-card-title>
      <v-row no-gutters>
        <v-col>
          <v-card-text> Launch Date: {{ launchDate }} </v-card-text>
          <v-card-text>
            Rocket Type: {{ launch.rocket.rocket_type }}
          </v-card-text>
          <v-card-text>
            Landing Intent:
            {{
              landingIntent(launch.rocket.first_stage.cores[0].landing_intent)
            }}
          </v-card-text>
        </v-col>
        <v-col>
          <v-card-text>
            Payload Type:
            {{ launch.rocket.second_stage.payloads[0].payload_type }}
          </v-card-text>
          <v-card-text>
            Nationality:
            {{ launch.rocket.second_stage.payloads[0].nationality }}
          </v-card-text>
          <v-card-text>
            Payload Mass:
            {{ launch.rocket.second_stage.payloads[0].payload_mass_kg }} Kg
          </v-card-text>
        </v-col>
      </v-row>
    </v-card>
  </v-timeline-item>
</template>

<script>
import moment from "moment";
export default {
  props: ["launch", "num"],
  methods: {
    showDetails() {
      if (this.details === false) {
        this.details = true;
      } else {
        this.details = false;
      }
    },
    landingIntent(landing) {
      if (landing) {
        return "Successful";
      } else {
        return "Not intended";
      }
    },
    successfulLaunch() {
      if (this.launch.launch_success) {
        return "launched successful";
      } else {
        return "launch failed";
      }
    },
  },
  computed: {
    launchDate() {
      return moment(this.launch.launch_date_local).format("DD.MM.YYYY");
    },
    changeColor() {
      if (this.num % 2) {
        return "#7C0A02";
      } else return "#F1BC31";
    },
  },
  data: () => ({ details: false }),
};
</script>

<style></style>
