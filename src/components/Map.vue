<template>
    <div>
        <Mapbox 
            :access-token="key"
            :map-options="{
                style: 'mapbox://styles/mapbox/light-v9',
                center: [-5, 54],
                zoom: 5,
            }" 
        />
    </div>
</template>

<script>
import Mapbox from 'mapbox-gl-vue'

export default {
  components: { Mapbox },

  data() {
      return {
          key: process.env.VUE_APP_MAPBOX_KEY
      }
  }
}
</script>

<style lang="scss" scoped>
#map {
  width: 100%;
  height: 100vh;
}
</style>