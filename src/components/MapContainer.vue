<template>
  <div id="map" ref="mapContainer" class="map-container">
  </div>
  <div v-if="selectedFeature" class="feature-card">
    <h3>{{ selectedFeature.title }}</h3>
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
      mapboxgl.accessToken =
        "YPUR_MAPBOC_ACESS_TOKEN"; // replace with your actual token
      this.map = new mapboxgl.Map({
        container: this.$refs.mapContainer,
        style: "mapbox://styles/mapbox/streets-v11",
        center: [69.18, 34.52],
        zoom: 6,
      });

      this.map.on("load", () => {
        this.map.setCenter([69.18, 34.52]);
        this.map.setZoom(6  );
        this.loadMapLayers();
      });
    },

    loadMapLayers() {
      // ---- Define the array of point data close to New Delhi, India ----
      const pointsData = [
        { id: 1, coordinates: [71.52, 36.73], title: "Fayzabad", description: "Capital of Badakhshan" },
        { id: 2, coordinates: [63.13, 35.17], title: "Qala i Naw", description: "Capital of Badghis" },
        { id: 3, coordinates: [68.71, 35.85], title: "Pul-e Khomri", description: "Capital of Baghlan" },
        { id: 4, coordinates: [67.07, 36.75], title: "Mazar-i-Sharif", description: "Capital of Balkh" },
        { id: 5, coordinates: [67.82, 34.82], title: "Bamyan", description: "Capital of Bamyan" },
        { id: 6, coordinates: [66.15, 33.72], title: "Nili", description: "Capital of Daykundi" },
        { id: 7, coordinates: [62.11, 32.37], title: "Farah", description: "Capital of Farah" },
        { id: 8, coordinates: [64.47, 36.2], title: "Maymana", description: "Capital of Faryab" },
        { id: 9, coordinates: [68.42, 33.55], title: "Ghazni", description: "Capital of Ghazni" },
        { id: 10, coordinates: [64.23, 34.52], title: "Ferozkoh", description: "Capital of Ghor" },
        { id: 11, coordinates: [64.38, 31.61], title: "Lashkargah", description: "Capital of Helmand" },
        { id: 12, coordinates: [62.17, 34.34], title: "Herat", description: "Capital of Herat" },
        { id: 13, coordinates: [65.75, 36.93], title: "Sheberghan", description: "Capital of Jowzjan" },
        { id: 14, coordinates: [69.18, 34.52], title: "Kabul", description: "Capital of Kabul" },
        { id: 15, coordinates: [65.70, 31.61], title: "Kandahar", description: "Capital of Kandahar" },
        { id: 16, coordinates: [69.62, 34.98], title: "Mahmud-e-Raqi", description: "Capital of Kapisa" },
        { id: 17, coordinates: [69.93, 33.33], title: "Khost", description: "Capital of Khost" },
        { id: 18, coordinates: [71.04, 34.64], title: "Asadabad", description: "Capital of Kunar" },
        { id: 19, coordinates: [68.87, 36.73], title: "Kunduz", description: "Capital of Kunduz" },
        { id: 20, coordinates: [70.20, 34.69], title: "Mehtar Lam", description: "Capital of Laghman" },
        { id: 21, coordinates: [69.22, 34.02], title: "Pul-e-Alam", description: "Capital of Logar" },
        { id: 22, coordinates: [70.45, 34.43], title: "Jalalabad", description: "Capital of Nangarhar" },
        { id: 23, coordinates: [63.13, 30.97], title: "Zaranj", description: "Capital of Nimroz" },
        { id: 24, coordinates: [70.79, 35.42], title: "Parun", description: "Capital of Nuristan" },
        { id: 25, coordinates: [69.36, 33.38], title: "Gardez", description: "Capital of Paktia" },
        { id: 26, coordinates: [68.47, 32.91], title: "Sharana", description: "Capital of Paktika" },
        { id: 27, coordinates: [69.52, 35.01], title: "Bazarak", description: "Capital of Panjshir" },
        { id: 28, coordinates: [69.16, 34.98], title: "Charikar", description: "Capital of Parwan" },
        { id: 29, coordinates: [67.35, 36.27], title: "Aybak", description: "Capital of Samangan" },
        { id: 30, coordinates: [66.30, 35.69], title: "Sar-e Pol", description: "Capital of Sar-e Pol" },
        { id: 31, coordinates: [69.62, 36.73], title: "Taloqan", description: "Capital of Takhar" },
        { id: 32, coordinates: [65.75, 33.32], title: "Tarin Kot", description: "Capital of Urozgan" },
        { id: 33, coordinates: [68.80, 34.40], title: "Maidan Shahr", description: "Capital of Wardak" },
        { id: 34, coordinates: [66.99, 32.62], title: "Qalat", description: "Capital of Zabul" }
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

      // ---- Adding Polygon Layers for Afghanistan States with Different Colors ----
      const afghanistanProvinces = [
      { name: "Badakhshan", coordinates: [[70.758, 37.285], [71.320, 36.738], [70.873, 36.631], [70.758, 37.285]], color: "#f28c28" },
      { name: "Badghis", coordinates: [[63.2, 35.2], [63.4, 35.4], [63.3, 35.5], [63.2, 35.2]], color: "#7e57c2" },
      { name: "Baghlan", coordinates: [[68.7, 36.0], [69.0, 36.4], [68.8, 36.6], [68.7, 36.0]], color: "#66bb6a" },
      { name: "Balkh", coordinates: [[67.1, 36.8], [67.3, 37.0], [67.2, 37.2], [67.1, 36.8]], color: "#42a5f5" },
      { name: "Bamyan", coordinates: [[67.5, 34.9], [67.7, 35.1], [67.6, 35.3], [67.5, 34.9]], color: "#ef5350" },
      { name: "Daykundi", coordinates: [[66.3, 33.7], [66.5, 33.9], [66.4, 34.1], [66.3, 33.7]], color: "#ab47bc" },
      { name: "Farah", coordinates: [[62.1, 32.4], [62.3, 32.6], [62.2, 32.8], [62.1, 32.4]], color: "#29b6f6" },
      { name: "Faryab", coordinates: [[64.5, 35.9], [64.7, 36.1], [64.6, 36.3], [64.5, 35.9]], color: "#66bb6a" },
      { name: "Ghazni", coordinates: [[68.4, 33.5], [68.6, 33.7], [68.5, 33.9], [68.4, 33.5]], color: "#ff7043" },
      { name: "Ghor", coordinates: [[64.4, 34.3], [64.6, 34.5], [64.5, 34.7], [64.4, 34.3]], color: "#ec407a" },
      { name: "Helmand", coordinates: [[64.0, 31.7], [64.2, 31.9], [64.1, 32.1], [64.0, 31.7]], color: "#26a69a" },
      { name: "Herat", coordinates: [[61.4, 34.3], [61.6, 34.4], [61.5, 34.7], [61.4, 34.3]], color: "#8d6e63" },
      { name: "Jowzjan", coordinates: [[65.2, 36.6], [65.4, 36.8], [65.3, 37.0], [65.2, 36.6]], color: "#78909c" },
      { name: "Kabul", coordinates: [[69.15, 34.5], [69.2, 34.8], [69.4, 34.7], [69.15, 34.5]], color: "#d4e157" },
      { name: "Kandahar", coordinates: [[65.7, 31.6], [65.8, 31.8], [66.0, 31.9], [65.7, 31.6]], color: "#ffca28" },
      { name: "Kapisa", coordinates: [[69.3, 34.9], [69.5, 35.1], [69.4, 35.3], [69.3, 34.9]], color: "#ffa726" },
      { name: "Khost", coordinates: [[69.8, 33.3], [70.0, 33.5], [69.9, 33.7], [69.8, 33.3]], color: "#ab47bc" },
      { name: "Kunar", coordinates: [[70.1, 34.8], [70.3, 35.0], [70.2, 35.2], [70.1, 34.8]], color: "#29b6f6" },
      { name: "Kunduz", coordinates: [[68.9, 36.6], [69.1, 36.8], [69.0, 37.0], [68.9, 36.6]], color: "#42a5f5" },
      { name: "Laghman", coordinates: [[70.0, 34.6], [70.2, 34.8], [70.1, 35.0], [70.0, 34.6]], color: "#ec407a" },
      { name: "Logar", coordinates: [[69.2, 33.9], [69.4, 34.1], [69.3, 34.3], [69.2, 33.9]], color: "#ff7043" },
      { name: "Nangarhar", coordinates: [[70.4, 34.4], [70.6, 34.6], [70.5, 34.8], [70.4, 34.4]], color: "#78909c" },
      { name: "Nimroz", coordinates: [[61.9, 31.0], [62.1, 31.2], [62.0, 31.4], [61.9, 31.0]], color: "#8d6e63" },
      { name: "Nuristan", coordinates: [[70.8, 35.3], [71.0, 35.5], [70.9, 35.7], [70.8, 35.3]], color: "#f28c28" },
      { name: "Paktia", coordinates: [[69.4, 33.2], [69.6, 33.4], [69.5, 33.6], [69.4, 33.2]], color: "#7e57c2" },
      { name: "Paktika", coordinates: [[68.1, 32.6], [68.3, 32.8], [68.2, 33.0], [68.1, 32.6]], color: "#66bb6a" },
      { name: "Panjshir", coordinates: [[69.7, 35.0], [69.9, 35.2], [69.8, 35.4], [69.7, 35.0]], color: "#ab47bc" },
      { name: "Parwan", coordinates: [[69.1, 35.1], [69.3, 35.3], [69.2, 35.5], [69.1, 35.1]], color: "#42a5f5" },
      { name: "Samangan", coordinates: [[67.3, 36.3], [67.5, 36.5], [67.4, 36.7], [67.3, 36.3]], color: "#ef5350" },
      { name: "Sar-e Pol", coordinates: [[65.8, 35.6], [66.0, 35.8], [65.9, 36.0], [65.8, 35.6]], color: "#29b6f6" },
      { name: "Takhar", coordinates: [[69.3, 36.7], [69.5, 36.9], [69.4, 37.1], [69.3, 36.7]], color: "#ec407a" },
      { name: "Urozgan", coordinates: [[65.0, 33.5], [65.2, 33.7], [65.1, 33.9], [65.0, 33.5]], color: "#ff7043" },
      { name: "Wardak", coordinates: [[68.6, 34.4], [68.8, 34.6], [68.7, 34.8], [68.6, 34.4]], color: "#78909c" },
      { name: "Zabul", coordinates: [[67.1, 32.8], [67.3, 33.0], [67.2, 33.2], [67.1, 32.8]], color: "#8d6e63" },
    ];

    afghanistanProvinces.forEach((province, index) => {
      this.map.addSource(`province-${index}`, {
        "type": "geojson",
        "data": {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [province.coordinates]
          }
        }
      });

      this.map.addLayer({
        "id": `province-layer-${index}`,
        "type": "fill",
        "source": `province-${index}`,
        "layout": {},
        "paint": {
          "fill-color": province.color,
          "fill-opacity": 0.6
        }
      });

      this.map.addLayer({
        "id": `province-border-${index}`,
        "type": "line",
        "source": `province-${index}`,
        "layout": {},
        "paint": {
          "line-color": "#000000",
          "line-width": 1
        }
      });
    });

      // Map click event to update selected feature details
      this.map.on("click", (event) => {
        const features = this.map.queryRenderedFeatures(event.point);
        if (features.length) {
          this.selectedFeature = features[0].properties;
        }
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
