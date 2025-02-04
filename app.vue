<template>
  <div style="height:100vh; width:100vw">
    <LMap
      ref="map"
      :zoom="12"
      :max-zoom="18"
      :center="[47.21322, -1.559482]"
      @ready="onMapReady"
    >
      <LTileLayer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        attribution="&amp;copy; <a href=&quot;https://www.openstreetmap.org/&quot;>OpenStreetMap</a> contributors"
        layer-type="base"
        name="OpenStreetMap"
      />
      <LMarker :lat-lng="[47.21322, -1.559482]" />
      <LMarker :lat-lng="[47.22322, -1.569482]" />
      <LMarker :lat-lng="[47.23322, -1.579482]" />
      <LMarker :lat-lng="[47.24322, -1.589482]" />
      <LMarker :lat-lng="[47.25322, -1.599482]" />
    </LMap>
  </div>
</template>

<script setup>
import L from 'leaflet'
import { ref } from 'vue'

const map = ref(null)

/**
 * Return all markers in the current view
 */
const getMarkersInView = () => {
  var markers = [];
  map.value.leafletObject.eachLayer(function(layer) {
    if(layer instanceof L.Marker) {
      if(map.value.leafletObject.getBounds().contains(layer.getLatLng())) {
        markers.push(layer);
      }
    }
  });
  return markers;
}

// When the map is ready
const onMapReady = () => {
  // First call is done by hand on map initialization
  const markers = getMarkersInView();
  console.log(markers);
  // Add a listener on 'moveend' event (user has either zoomed or panned the map)
  map.value.leafletObject.on('moveend', () => {
    const markers = getMarkersInView();
    console.log(markers);
  });
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
</style>
