<template>
<!-- @TODO: toggle easily between directions of traffic -->
  <aside id="sidebar">
    <div class="sidebar__toggle" 
      :class="{'sidebar__toggle--open': openSidebar}" 
      @click="toggleSidebar"
    >
      <span/>
      <span/>
      <span/>
    </div>
    <div v-if="hasData" 
      class="sidebar__body-wrapper"
      :class="{'sidebar__body-wrapper--viewable': openSidebar }"
    >
      <h1>
        {{ roadData.road_name }} ({{ displayDirection(roadData.direction_of_travel) }})
      </h1>

      <hr />
      <!-- @TODO: toggle between views to display types of vehicle against total vehicles of that type -->
      <ul>
        <li v-for="(key, data) in roadData"
          :key="data"
        >
          
          {{ displayText(data) }}:
          <strong>
            {{ key }}
          </strong>
        </li>
      </ul>
    </div>
    <div v-else
      class="sidebar__body-wrapper"
      :class="{'sidebar__body-wrapper--viewable': openSidebar }"
    >
      <h1>
        Welcome
      </h1>
      <hr />
      <p>
        To view datasets for roads click a marker on the map
      </p>
    </div>
  </aside>
</template>

<script>
import Vue from 'vue'
export default Vue.extend({
    name: 'Sidebar',

    props: {
      roadData: {
        type: Object,
        required: true,
      },

      openSidebar: {
        type: Boolean,
        default: true,
      }
    },

    computed: {
      hasData() {
        return Object.keys(this.roadData).length;
      }
    },

    methods: {
      toggleSidebar() {
        this.$emit('toggle');
      },

      displayText(string) {
        const words = string.split('_');
        return words.map(word => word.substring(0, 1).toUpperCase() + word.substring(1)).join(' ');
      },

      displayDirection(string) {
        if (string === 'N') {
          return 'Northbound';
        }
        if (string === 'S') {
          return 'Southbound';
        }
        if (string === 'E') {
          return 'Eastbound';
        }
        if (string === 'W') {
          return 'Westbound';
        }
      }
    }
  })
</script>

<style lang="scss" scoped>
@import '../styles/_colours';

  #sidebar {
    background: $opaqueWhite;
    box-shadow: $dropShadow;
    max-width: 350px;
    width: 100%;
    height: 100vh;
    position: absolute;
    padding: 1rem;
    transition: all .2s ease;
    overflow-y: auto;
    z-index: 1;

    &.sidebar {
      &--show {
        left: 0;
      }

      &--hidden {
        left: -300px;
      }
    }

    ul {
      padding: 0;
      list-style: none;
      
      li {
        margin-bottom: 1rem;
        cursor: pointer;
      }
    }
  }

  hr {
    max-width: 50px;
  }

  .sidebar {
    &__toggle {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      position: absolute;
      right: 0;
      height: 50px;
      width: 50px;
      border-bottom-right-radius: 5px;
      border-top-right-radius: 5px;
      overflow: hidden;
      cursor: pointer;
      z-index: 3;

      span {
        width: 30px;
        height: 2px;
        display: block;
        margin-bottom: 5px;
        background: lighten($fontGray, 50%);
        transition: all .2s ease;
        transform: rotate(0) translate(0, 0);

        &:last-child {
          margin-bottom: 0;
        }
      }

      &--open {
        span:nth-child(1) {
          transform: rotate(45deg) translate(5px, 5px);
        }

        span:nth-child(2) {
          margin-right: -100px;
        }

        span:nth-child(3) {
          transform: rotate(-45deg) translate(5px, -5px);
        }
      }
    }

    &__body-wrapper {
      height: 90vh;
      overflow-y: hidden;

      h1, hr, p, li {
        opacity: 0;
        margin-left: -100px;
      }

      h2 {
        font-size: 1.1rem;
        color: lighten($fontGray, 30%);
      }

      &--viewable {
        height: auto;
        overflow-y: auto;

        h1, hr, p, li {
          opacity: 1;
          margin-left: 0;
          transition-property: all;
          transition-timing-function: ease-in-out;
        }

        h1 {
          transition-duration: 0.2s;
        }

        hr {
          transition-duration: 0.25s;
        }

        p {
          transition-duration: 0.3s;
        }
        @for $i from 1 through 32 {
          li:nth-child(#{$i}) {
            transition-duration: $i / 100 + 0.3s; 
          }
        }
      }
    }
  }
</style>