<template>
    <div>
        <Mapbox 
            :access-token="key"
            :map-options="{
                style: 'mapbox://styles/mapbox/light-v9',
                center: [-5, 51],
                zoom: 7,
            }" 
            :geolocate-control="{
              show: true,
              position: 'top-right',
            }"
            @map-load="loaded"
            @map-click:points="clicked"
        />
    </div>
</template>

<script>
import Mapbox from 'mapbox-gl-vue';

export default {
  components: { Mapbox },

  props: {
    selected: {
      type: String,
      default: ''
    },

    roads: {
      type: Object,
      required: true,
    }
  },

  data() {
      return {
          key: process.env.VUE_APP_MAPBOX_KEY,
      }
  },

  computed: {
    latLongCoordinates() {
      const features = [];
      Object.values(this.roads).map(road => {
        road.forEach(roadSegment => {
          const feature = {
            type: 'Feature',
            geometry: {
              type: 'Point',
              coordinates: [roadSegment.longitude, roadSegment.latitude]
            },
            properties: {
              title: roadSegment.road_name,
              description: roadSegment,
            },
          }
          features.push(feature);
        })
      });
      return features;
    }
  },

   methods: {
     clicked(map, e) {
       this.$emit('popup', JSON.parse(e.features[0].properties.description));
     },

    loaded(map) {
      //@TODO: map out the coordinates for the road between the start and end junctions
      // Add animation to show traffic flow direction
      map.addLayer({
        id: 'points',
        type: 'circle',
        source: {
          type: 'geojson',
          data: {
            type: 'FeatureCollection',
            features: [
              ...this.latLongCoordinates,
            ]
          }
        },
        paint: {
          //@TODO: change circle radius based on zoom
          // and change colour based on amount of traffic / on click (ie focus)
          'circle-radius': 5,
          'circle-color': '#ffb000'
        }
      })
   },
   }
}
</script>

<style lang="scss">
    #map {
        width: 100%;
        height: 100vh;
    }
</style>