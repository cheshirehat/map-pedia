<template>
  <div class="map-contents">
    <div id="map" ref="map"></div>
  </div>
</template>

<script>
import EventBus from '../service/EventBus.js'

export default {
  props: ['center', 'plans'],
  data () {
    return {
      map: null,
      marker: [],
    }
  },
  created () {
    EventBus.$on('search-geocode', this.searchGeocode)
  },
  mounted () {
    const map = this.$refs.map
    this.map = new window.google.maps.Map(map, {
      center: this.center,
      zoom: 16
    })

    window.google.maps.event.addListener(this.map, 'click', this.clicker)

  },
  methods: {
    clicker (map) {
      this.getAddress(map.latLng.lat(), map.latLng.lng())
    },
    getAddress (lat, lng) {
      const latLng = new window.google.maps.LatLng(lat, lng)
      const geocoder = new window.google.maps.Geocoder()
      geocoder.geocode({ latLng: latLng },(result, status) => {
        if (status == window.google.maps.GeocoderStatus.OK) {
          const data = result[0].address_components[0].long_name
          EventBus.$emit('search-wiki', data)
          this.resetMarker(latLng)
        }
      })
    },
    searchGeocode (address) {
      const geocoder = new window.google.maps.Geocoder()
      geocoder.geocode({ 'address': address }, (result, status) => {
        if (status == window.google.maps.GeocoderStatus.OK) {
          const searchLocate = {
            lat: result[0].geometry.location.lat(),
            lng: result[0].geometry.location.lng()
          }
          this.map.setCenter(new window.google.maps.LatLng(searchLocate))
          this.resetMarker(searchLocate)
        }
      })
    },
    resetMarker (latLng) { 
      this.marker.forEach(marker => marker.setMap(null))
      this.marker = []
      const marker = new window.google.maps.Marker({
        position: latLng,
        map: this.map,
        animation: window.google.maps.Animation.DROP
      })
      this.marker.push(marker)
    }
  }
}
</script>


<style>
#map {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #ccc;
}
</style>
