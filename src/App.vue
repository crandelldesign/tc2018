<template>
  <div id="app">
    <h1>Traverse City 2018</h1>
    <div class="google-map" id="map"></div>
    <p class="text-center"><small>Zoom out to see more.</small></p>
    <div v-for="(attraction, index) in attractions" v-bind:key="attraction.id">
      <h2>{{ index + 1 }}. {{ attraction.name }}</h2>
      <address>{{ attraction.address }}</address>
      <p>{{ attraction.description }}</p>
      <hr />
    </div>
  </div>
</template>

<script>
import data from "./assets/data.json";
const google = window.google;
export default {
  name: "app",
  data() {
    return {
      attractions: data.attractions,
      mapName: "map",
      map: null,
      bounds: null,
      markers: []
    };
  },
  created() {},
  mounted() {
    this.createMap();
  },
  methods: {
    createMap: function() {
      this.bounds = new google.maps.LatLngBounds();
      const element = document.getElementById("map");
      const options = {
        center: { lat: 44.762261, lng: -85.620629 },
        zoom: 15
      };
      this.map = new google.maps.Map(element, options);
      // Add Markers
      //let markers = [];
      this.attractions.forEach((attraction, index) => {
        let infowindow = null;
        const position = new google.maps.LatLng(attraction.lat, attraction.lng);
        const marker = new google.maps.Marker({
          position,
          map: this.map,
          label: (index + 1).toString()
        });
        google.maps.event.addListener(marker, "click", function() {
          let map;
          infowindow = new google.maps.InfoWindow();
          infowindow.setContent(
            "<div><strong>" +
              attraction.name +
              "</strong><br>" +
              attraction.address +
              "<br>" +
              attraction.description +
              "</div>"
          );
          infowindow.open(map, this);
        });
        this.markers.push(marker);
      });
    }
  }
};
</script>

<style lang="scss">
// TC 2018
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin: 0 auto;
  max-width: 1280px;
}
h1,
h2 {
  font-weight: normal;
}
p {
  margin-top: 0;
  margin-bottom: 10px;
}
.text-center {
  text-align: center;
}
.google-map {
  width: 100%;
  height: 500px;
  margin: 0 auto;
  background: gray;
}
</style>
