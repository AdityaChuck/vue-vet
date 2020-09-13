<template>
  <div id="main-app" className="container">
    <div class="row justify-content-center">
      <AddAppointment @add="addItem" />
      <SearchAppointment @searchRecords="searchAppointments" 
        :filterKey="filterKey" :filterDir="filterDir" 
        @requestkey="changeKey" @requestdir="changeDir"
      />
      <AppointmentList
        :appointments="filteredApts"
        @remove="removeItem"
        @edit="editItem"
      />
    </div>
  </div>
</template>

<script>
import AddAppointment from "./components/AddAppointment";
import AppointmentList from "./components/AppointmentList";
import SearchAppointment from "./components/SearchAppointment";

import axios from "axios";
import _ from "lodash";

export default {
  name: "MainApp",
  data: () => ({
    appointments: [],
    aptIndex: 0,
    searchTerms: "",
    filterKey: "petName",
    filterDir: "asc",
  }),
  components: {
    AppointmentList,
    AddAppointment,
    SearchAppointment,
  },
  computed: {
    searchApts: function() {
      return this.appointments.filter(
        (item) =>
          String(item.petName)
            .toLowerCase()
            .match(String(this.searchTerms).toLowerCase()) ||
          String(item.petOwner)
            .toLowerCase()
            .match(String(this.searchTerms).toLowerCase()) ||
          String(item.aptNotes)
            .toLowerCase()
            .match(String(this.searchTerms).toLowerCase())
      );
    },
    filteredApts: function() {
      return _.orderBy(
        this.searchApts,
        (item) => {
          return item[this.filterKey].toLowerCase()
        },
        this.filterDir
      )
    },
  },
  methods: {
    removeItem: function(apt) {
      this.appointments = _.without(this.appointments, apt);
    },
    editItem: function(id, field, text) {
      const aptIndex = _.findIndex(this.appointments, {
        aptId: id,
      });
      this.appointments[aptIndex][field] = text;
    },
    addItem: function(apt) {
      apt.aptId = this.aptIndex;
      this.aptIndex++;
      this.appointments = [apt, ...this.appointments];
    },
    searchAppointments: function(terms) {
      this.searchTerms = terms;
    },
    changeKey: function(value){
      this.filterKey = value
    },
    changeDir: function(value){
      this.filterDir = value
    }
  },
  mounted() {
    axios.get("./data/appointments.json").then((response) => {
      this.appointments = response.data.map((item) => {
        item.aptId = this.aptIndex;
        this.aptIndex++;
        return item;
      });
    });
  },
};
</script>
