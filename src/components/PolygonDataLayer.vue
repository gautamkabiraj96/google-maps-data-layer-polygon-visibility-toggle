<template>
  <div>
    <div class="header-margins">
      Google Maps Polygon |
      <small>www.nightprogrammer.com</small>
      <div style="margin-top: 20px">
        <button @click="togglePolygonVisibility">
          {{ isPolygonVisible ? "Hide" : "Show" }} polygon
        </button>
      </div>
    </div>
    <div id="data-layers-map" class="map-margins"></div>
  </div>
</template>

<script>
import loadGoogleMapsApi from "load-google-maps-api";
import { gMapsApiKey, polygonCoords } from "./../constants";

export default {
  name: "ToggleDataLayerPolygonVisibility",
  data() {
    return {
      map: null,
      isPolygonVisible: true,
    };
  },
  created() {
    this.polygonCoords = polygonCoords;
  },
  mounted() {
    loadGoogleMapsApi({
      key: gMapsApiKey,
      libraries: ["drawing", "geometry"],
    }).then(async () => {
      const mapZoom = 14;
      const { google } = window;
      const mapOptions = {
        zoom: mapZoom,
        mapTypeId: google.maps.MapTypeId.HYBRID,
        center: new google.maps.LatLng({ lat: 23, lng: 57 }),
        mapTypeControl: true,
        streetViewControl: false,
        mapTypeControlOptions: {
          position: google.maps.ControlPosition.BOTTOM_LEFT,
        },
      };
      this.map = new google.maps.Map(
        document.getElementById("data-layers-map"),
        mapOptions
      );

      const coordinates = this.polygonCoords;

      const tempBounds = new google.maps.LatLngBounds();

      for (let j = 0; j < coordinates.length; j++) {
        const x = {
          lat: coordinates[j].lat,
          lng: coordinates[j].lng,
        };
        const BoundLatLng = new google.maps.LatLng({
          lat: parseFloat(x.lat),
          lng: parseFloat(x.lng),
        });
        tempBounds.extend(BoundLatLng);
      }
      const centroid = tempBounds.getCenter();

      this.map.data.add({
        geometry: new google.maps.Data.Polygon([coordinates]),
      });

      this.map.setCenter(centroid);
    });
  },
  methods: {
    togglePolygonVisibility() {
      this.isPolygonVisible = !this.isPolygonVisible;
      this.map.data.setStyle({
        visible: this.isPolygonVisible,
      });
    },
  },
};
</script>

<style scoped>
.header-margins {
  margin-left: 40px;
  margin-top: 20px;
}

.map-margins {
  height: 400px;
  width: 600px;
  margin: 10px 40px;
}
button {
  background-color: hsla(160, 100%, 37%, 1);
  color: #fff;
  margin-right: 4px;
}
</style>