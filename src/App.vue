<template>
  <main>
    <Sidebar 
      :class="openSidebar ? 'sidebar--show' : 'sidebar--hidden'" 
      :openSidebar="openSidebar" :roadData="clickData"
      @toggle="openSidebar = !openSidebar"
    />
    <Map @popup="toggleSidebar" :roads="filterByRoadName" />

    <!-- @TODO: Show a graph to compare between two datasets side by side -->

  </main>
</template>

<script>
import Map from './components/Map';
import Sidebar from './components/Sidebar';

export default {
  name: 'app',

  components: {
    Map,
    Sidebar
  },

  data() {
    return {
      data: null,
      error: null,
      selected: null,
      openSidebar: false,
      clickData: {},
    }
  },

  computed: {
    filterByRoadName() {
      if (this.data) {
        return this.data.reduce((acc, obj) => {
          const name = obj.road_name;
          if (!acc[name]) {
            acc[name] = [];
          }
          acc[name].push(obj);
          return acc;
        }, {});
      }
      return {};
    }
  },

  created() {
      const url = '/api/average-annual-daily-flow-by-direction?filter[local_authority_id]=71';
      return fetch(url)
				.then(response => response.json())
				.then(result => this.data = result.data)
        .catch(err => this.error = err);
  },

  methods: {
    toggleSidebar(data) {
      this.clickData = data;
      if (!this.openSidebar) {
        this.openSidebar = true;
      }
    }
  }
}
</script>

<style lang="scss">
@import './styles/_colours';
  body {
    margin: 0;
    height: 100vh;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    font-size: 14px;
    color: $fontGray;
  }

  * {
    box-sizing: border-box;
  }

  .side-enter-active, .side-leave-active {
    transition: all .5s;
    left: 0 !important;
  }
  .side-enter, .side-leave-to /* .fade-leave-active below version 2.1.8 */ {
    left: -200px !important;
    transition: all .5s;
  }
</style>
