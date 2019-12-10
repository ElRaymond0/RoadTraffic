<template>
  <main>
    <Map />
  </main>
</template>

<script>
import Map from './components/Map';

export default {
  name: 'app',

  components: {
    Map
  },

  data() {
    return {
      data: null,
      error: null,
    }
  },

  computed: {
    filterByRoadName() {
      return this.data.reduce((acc, obj) => {
          const name = obj.road_name;
          if (!acc[name]) {
            acc[name] = [];
          }
          acc[name].push(obj);
          return acc;
        }, {});
    }
  },

  created() {
      const url = '/api/average-annual-daily-flow-by-direction?filter[local_authority_id]=71';
      return fetch(url)
				.then(response => response.json())
				.then(result => this.data = result.data)
        .catch(err => this.error = err);
  },
}
</script>

<style lang="scss">
  body {
    margin: 0;
  }
</style>
