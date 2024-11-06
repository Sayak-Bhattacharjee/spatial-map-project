<template>
  <div id="map" ref="mapContainer" class="map-container"></div>
  <div v-if="selectedFeature" class="feature-card">
    <h3>{{ selectedFeature.name }}</h3>
    <p>{{ selectedFeature.description }}</p>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";

export default {
  name: "MapContainer",
  data() {
    return {
      map: null,
      selectedFeature: null,
    };
  },
  mounted() {
    this.initializeMap();
  },
  methods: {
    initializeMap() {
      mapboxgl.accessToken = "pk.add_access_token.BqrqSgTnZMgOb1ZYygSK4Q"; // Replace with your actual token
      this.map = new mapboxgl.Map({
        container: this.$refs.mapContainer,
        style: "mapbox://styles/mapbox/streets-v11",
        center: [69.18, 34.52],
        zoom: 6,
      });

      this.map.on("load", () => {
        this.loadProvinces();
        this.loadPoints();
      });
    },

    loadProvinces() {
      
      this.map.addSource("afghanistan-provinces", {
        type: "geojson",
        data: "/afghanistan_provinces.geojson",
      });

      // Add fill layer with different colors for each province
      this.map.addLayer({
        id: "provinces-fill",
        type: "fill",
        source: "afghanistan-provinces",
        paint: {
          "fill-color": [
            "match",
            ["get", "name"],
            "Badakhshan",
            "#f28c28",
            "Badghis",
            "#7e57c2",
            "Baghlan",
            "#66bb6a",
            "Balkh",
            "#42a5f5",
            "Bamyan",
            "#ef5350",
            // rest ofthe provinces are not used.
            /* fallback color if province name is not matched */ "#d4e157",
          ],
          "fill-opacity": 0.6,
        },
      });

      // Add a border around each province
      this.map.addLayer({
        id: "provinces-border",
        type: "line",
        source: "afghanistan-provinces",
        paint: {
          "line-color": "#000",
          "line-width": 1,
        },
      });

      // Set up click event to display information for each province
      this.map.on("click", "provinces-fill", (e) => {
        const feature = e.features[0];
        this.selectedFeature = {
          name: feature.properties.name,
          description:
            feature.properties.description || "No description available",
        };
      });
    },

    loadPoints() {
      const pointsData = [
        {
          id: 1,
          coordinates: [71.52, 36.73],
          title: "Fayzabad",
          description: "Capital of Badakhshan",
        },
        {
          id: 2,
          coordinates: [63.13, 35.17],
          title: "Qala i Naw",
          description: "Capital of Badghis",
        },
        {
          id: 3,
          coordinates: [68.71, 35.85],
          title: "Pul-e Khomri",
          description: "Capital of Baghlan",
        },
      ];

      pointsData.forEach((point) => {
        const marker = new mapboxgl.Marker()
          .setLngLat(point.coordinates)
          .setPopup(
            new mapboxgl.Popup().setHTML(
              `<h3>${point.title}</h3><p>${point.description}</p>`
            )
          )
          .addTo(this.map);

        marker.getElement().addEventListener("click", () => {
          this.selectedFeature = {
            title: point.title,
            description: point.description,
          };
        });
      });
    },
  },
};
</script>

<style>
.map-container {
  width: 100%;
  height: 100vh;
}
.feature-card {
  position: absolute;
  top: 10px;
  left: 10px;
  background: white;
  padding: 10px;
  border-radius: 4px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}
</style>
