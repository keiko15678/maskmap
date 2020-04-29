<template>
  <l-map :zoom="zoom" :center="center">
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    <l-marker
      v-for="(item, index) in maskData"
      :key="index"
      :lat-lng="convertCoordinateFormat(item.geometry.coordinates)"
    >
      <l-tooltip
        :options="{
          permanent:
            parseInt(manualCenter[0]) ===
              parseInt(item.geometry.coordinates[0]) &&
            parseInt(manualCenter[1]) === parseInt(item.geometry.coordinates[1])
        }"
      >
        <h5 class="font-weight-bold text-primary">{{ item.properties.name }}</h5>
        <p class="mt-n2">{{ item.properties.phone }}</p>
        <p class="mt-n2">{{ item.properties.address }}</p>
        <p class="mt-n2" style="font-size: 1.1rem">
          <span class="mr-2 d-block text-danger"
            ><strong>成人口罩</strong>：{{ item.properties.mask_adult }}</span
          >
          <span class="text-danger"
            ><strong>兒童口罩</strong>：{{ item.properties.mask_child }}</span
          >
        </p>
      </l-tooltip>
    </l-marker>
  </l-map>
</template>

<script>
import { latLng } from 'leaflet'
export default {
  props: {
    maskData: {
      type: Array,
      required: true
    },
    manualCenter: {
      type: Array,
      default: null
    }
  },
  data() {
    return {
      zoom: 16,
      center: latLng(25.0475613, 121.5173399),
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
    }
  },
  methods: {
    convertCoordinateFormat(coordinate) {
      return latLng(coordinate[1], coordinate[0])
    }
  },
  watch: {
    maskData: {
      handler() {
        this.center = this.convertCoordinateFormat(
          this.maskData[0].geometry.coordinates
        )
        this.zoom = 16
      }
    },
    manualCenter: {
      handler() {
        this.center = this.convertCoordinateFormat(this.manualCenter)
        this.zoom = 18
      }
    }
  }
}
</script>

<style></style>
