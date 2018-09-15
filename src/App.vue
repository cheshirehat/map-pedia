<template>
  <div id="app">
    <nav class="navbar nav-color" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <a class="navbar-item" href="http://localhost:8080">
          MapPedia
        </a>

        <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>
    </nav>

    <div class="columns">
      <div class="column is-7">
        <Map v-bind:center="center" v-bind:plans="plans" v-on:update-map-center="updateMapCenter"></Map>
      </div>
      <div class="column is-5">
        <Search></Search>
        <Wiki></Wiki>
      </div>
    </div>
  </div>
</template>

<script>
import Map from './components/Map.vue'
import Wiki from './components/Wiki.vue'
import Search from './components/Search.vue'

export default {
  name: 'app',
  components: {
    Map,
    Wiki,
    Search
  },
  data () {
    return {
      plans: [],
      center: { lat: 35.4437078, lng: 139.6380256 }
    }
  },
  methods: {
    updateMapCenter (center) {
      this.center = center
    }
  }
}
</script>

<style>
nav.navbar {
  background-color: #424242;
}
.navbar-brand a {
  color: aliceblue;
}
.map-contents {
  position: relative;
  padding-bottom: 90%;
  height: 0;
  overflow: hidden;
}
</style>
