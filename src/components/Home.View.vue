<template>
  <v-container>
    <v-progress-linear
      indeterminate
      v-show="this.isLoading"
    ></v-progress-linear>
    <v-alert type="error" v-show="errorMessage != ''">{{
      errorMessage
    }}</v-alert>
    <h3>Please select a station</h3>
    <v-autocomplete
      clearable
      :items="stations"
      item-text="Description"
      item-value="Description"
      placeholder="Station name"
      v-on:change="StationSelected"
    ></v-autocomplete>
    <v-divider></v-divider>
    <v-row>
      <v-col v-if="trains.length === 0">No trains to show.</v-col>
      <v-col v-else v-for="train in trains" :key="train.Id" md="4">
        <v-card class="mx-auto" outlined>
          <v-list-item three-line>
            <v-list-item-content>
              <div class="overline mb-4">
                {{ train.Code }} - {{ train.Direction }}
              </div>
              <v-list-item-title class="headline mb-1">
                From {{ train.Origin }} to {{ train.Destination }} ({{
                  train.DueIn
                }}
                mins)
              </v-list-item-title>
              <v-list-item-subtitle
                >Expected Arrival: {{ train.ExpectedArrival }} - Expected
                Departure: {{ train.ExpectedDepart }}</v-list-item-subtitle
              >
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "HomeView",
  created: function () {
    this.ListStations();
  },
  data: () => {
    return {
      isLoading: false,
      errorMessage: "",
      stations: [],
      trains: [],
    };
  },
  methods: {
    StationSelected: function (station) {
      this.isLoading = true;
      this.errorMessage = "";

      if (!station) {
        this.trains = [];
        return;
      }

      axios
        .get(`http://localhost:8000/api/trains/${station}`)
        .then((resp) => (this.trains = resp.data))
        .catch((err) => {
          this.trains = [];
          this.errorMessage = err.response?.data.error || err.message;
        })
        .finally(() => (this.isLoading = false));
    },
    ListStations: function () {
      this.isLoading = true;

      axios
        .get("http://localhost:8000/api/stations")
        .then((resp) => {
          this.stations = resp.data.sort((a, b) => {
            if (a.Description < b.Description) return -1;
            if (a.Description > b.Description) return 1;
            return 0;
          });
        })
        .catch((err) => {
          this.errorMessage = err.response?.data.error || err.message;
        })
        .finally(() => (this.isLoading = false));
    },
  },
};
</script>
