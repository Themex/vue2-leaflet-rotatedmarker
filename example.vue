<template>
  <v-map :zoom=10 :center="initialLocation">
    <v-icondefault></v-icondefault>
    <v-tilelayer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></v-tilelayer>
    <v-marker-cluster :options="clusterOptions">
      <v-rotated-marker v-for="l in locations" :key="l.id"
                        :lat-lng="l.latlng"
                        :icon="icon"
                        @click="handleMarkerClick(l)"
                        :rotationAngle="l.yaw">
      </v-rotated-marker>
    </v-marker-cluster>
  </v-map>
</template>

<script>
  import * as L from 'leaflet'
  import * as Vue2Leaflet from 'vue2-leaflet'
  import Vue2LeafletRotatedMarker from './Vue2LeafletRotatedMarker'
  import Vue2LeafletMarkerCluster from 'vue2-leaflet-markercluster'
  import 'leaflet.markercluster/dist/MarkerCluster.css'
  import 'leaflet.markercluster/dist/MarkerCluster.Default.css'
  import iconUrl from 'arrow-red.svg'
  import shadowUrl from 'leaflet/dist/images/marker-shadow.png'

  function rand(n) {
    let max = n + 0.1;
    let min = n - 0.1;
    return Math.random() * (max - min) + min;
  }

  export default {
    components: {
      'v-map': Vue2Leaflet.LMap,
      'v-tilelayer': Vue2Leaflet.LTileLayer,
      'v-icondefault': Vue2Leaflet.LIconDefault,
      'v-marker': Vue2Leaflet.LMarker,
      'v-rotated-marker': Vue2LeafletRotatedMarker,
      'v-marker-cluster': Vue2LeafletMarkerCluster
    },
    methods: {
      handleMarkerClick(l){
        l.yaw+=20;
      }
    },
    data () {
      let locations = [];
      for (let i = 0; i < 10; i++) {
        locations.push({
          id: i,
          yaw: 45,
          latlng: L.latLng(rand(-34.9205), rand(-57.953646)),
          text: 'Hola ' + i
        })
      }
      let icon = L.icon({
        iconUrl:iconUrl,
        iconSize: [30, 30],
        iconAnchor: [20, 20]
      });
      return {
        locations,
        icon,
        clusterOptions: {},
        initialLocation: L.latLng(-34.9205, -57.953646)
      }
    }
  }
</script>

<style>
  @import "~leaflet/dist/leaflet.css";
  html, body {
    height: 100%;
    margin: 0;
  }
</style>
