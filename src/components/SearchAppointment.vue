<template>
  <div class="col-12 col-md-10 col-lg-7">
    <div class="input-group my-3">
      <input
        id="SearchApts"
        placeholder="Search"
        type="text"
        class="form-control"
        aria-label="Search Appointments"
        v-model="searchTerm"
      />

      <div class="input-group-append">
        <button
          type="button"
          class="btn btn-primary dropdown-toggle"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false"
        >
          Sort by
          <span class="caret"></span>
        </button>

        <div class="dropdown-menu dropdown-menu-right">
          <a
            href="#"
            class="dropdown-item d-flex justify-content-between"
            id="petName"
            @click="$emit('requestkey', 'petName')"
          >
            Pet Name
            <font-awesome-icon icon="check" v-if="filterKey === 'petName'"/>
          </a>

          <a
            class="dropdown-item d-flex justify-content-between"
            href="#"
            id="aptDate"
            @click="$emit('requestkey', 'aptDate')"
          >
            Date
            <font-awesome-icon icon="check" v-if="filterKey === 'aptDate'" />
          </a>

          <a
            href="#"
            class="dropdown-item d-flex justify-content-between"
            id="ownerName"
            @click="$emit('requestkey', 'petOwner')"
          >
            Owner
            <font-awesome-icon icon="check" v-if="filterKey === 'petOwner'" />
          </a>

          <div class="dropdown-divider" role="separator"></div>

          <a
            class="dropdown-item d-flex justify-content-between"
            href="#"
            id="asc"
            @click="$emit('requestdir', 'asc')"
          >
            Asc
            <font-awesome-icon icon="check" v-if="filterDir === 'asc'"  />
          </a>

          <a
            class="dropdown-item d-flex justify-content-between"
            href="#"
            id="desc"
            @click="$emit('requestdir', 'desc')"
          >
            Desc
            <font-awesome-icon icon="check" v-if="filterDir === 'desc'" />
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

export default {
    name:"SearchAppointment",
    components:{
        FontAwesomeIcon
    },
    data: () => ({
      searchTerm:"",
    }),
    props:["filterKey", "filterDir"],
    watch: {
      searchTerm: function(){
        this.$emit("searchRecords", this.searchTerm)
      }
    }
}
</script>
