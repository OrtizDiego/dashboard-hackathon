<template>
  <gmap-map
    id="map"
    ref="gmap"
    :center="center"
    :zoom="13"
    :options="options"
    map-type-id="terrain"
  >
    <gmap-marker :position="center">
    </gmap-marker>
  </gmap-map>
</template>
<script>
  import {API_KEY} from './Maps/API_KEY'
  import Vue from 'vue'
  import * as VueGoogleMaps from 'vue2-google-maps'
  Vue.use(VueGoogleMaps, {
    load: {
      key: API_KEY,
      libraries: 'visualization', // necessary for heatmap layer
    }
  })
  export default {
    data () {
      return {
        center: {
          lat: 47.169753170157186, 
          lng: 13.106657266616821
        },
        options: {
          styles: [{
            'featureType': 'water',
            'stylers': [{'saturation': 43}, {'lightness': -11}, {'hue': '#0088ff'}]
          }, {
            'featureType': 'road',
            'elementType': 'geometry.fill',
            'stylers': [{'hue': '#ff0000'}, {'saturation': -100}, {'lightness': 99}]
          }, {
            'featureType': 'road',
            'elementType': 'geometry.stroke',
            'stylers': [{'color': '#808080'}, {'lightness': 54}]
          }, {
            'featureType': 'landscape.man_made',
            'elementType': 'geometry.fill',
            'stylers': [{'color': '#ece2d9'}]
          }, {
            'featureType': 'poi.park',
            'elementType': 'geometry.fill',
            'stylers': [{'color': '#ccdca1'}]
          }, {
            'featureType': 'road',
            'elementType': 'labels.text.fill',
            'stylers': [{'color': '#767676'}]
          }, {
            'featureType': 'road',
            'elementType': 'labels.text.stroke',
            'stylers': [{'color': '#ffffff'}]
          }, {'featureType': 'poi', 'stylers': [{'visibility': 'off'}]}, {
            'featureType': 'landscape.natural',
            'elementType': 'geometry.fill',
            'stylers': [{'visibility': 'on'}, {'color': '#b8cb93'}]
          }, {'featureType': 'poi.park', 'stylers': [{'visibility': 'on'}]}, {
            'featureType': 'poi.sports_complex',
            'stylers': [{'visibility': 'on'}]
          }, {'featureType': 'poi.medical', 'stylers': [{'visibility': 'on'}]}, {
            'featureType': 'poi.business',
            'stylers': [{'visibility': 'simplified'}]
          }]
        },
    };
  },
  mounted() {
    // Wait for the Google Maps API to be ready
    this.$refs.gmap.$mapPromise.then((map) => {
      // Now the google object is available and can be used to create a heatmap
      const heatmapData = [
        // Convert your data to google.maps.LatLng objects
        {location: new google.maps.LatLng(47.17, 13.11), weight: 3},
        {location: new google.maps.LatLng(47.16, 13.12), weight: 2},
        // ... more data
      ];

      const heatmap = new google.maps.visualization.HeatmapLayer({
        data: heatmapData,
        map: map,
        radius : 50
      });
    });
  }
}
</script>
<style>
  #map {
    min-height: calc(100vh - 123px);
  }
</style>
