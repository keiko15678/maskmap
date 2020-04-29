<template>
  <li class="list-group-item pointer">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title d-flex align-items-center">
          <strong class="text-primary">{{ store.properties.name }}</strong>
        </h5>
        <p class="card-text mt-n2" v-show="open">
          {{ store.properties.address }}
        </p>
        <p class="card-text mt-n2">
          <span class="mr-2 text-danger"
            ><strong class="text-dark">成人口罩: </strong
            >{{ store.properties.mask_adult }}</span
          >
          <span class="text-danger"
            ><strong class="text-dark">兒童口罩: </strong
            >{{ store.properties.mask_child }}</span
          >
        </p>
        <p class="card-text mt-n2" v-show="open">
          聯絡電話：{{ store.properties.phone }}
        </p>
        <p class="card-text mt-n2" v-show="open">
          備註：{{ store.properties.note }}
        </p>
        <p class="card-text mt-n2" v-show="open">
          <small class="text-secondary"
            >更新時間：{{ store.properties.updated }}</small
          >
        </p>
        <div class="d-flex">
          <span
            class="mr-2 btn btn-primary text-white"
            v-show="!open"
            @click="handleOpen"
            >詳情</span
          >
          <span
            class="mr-2 btn btn-primary text-white"
            v-show="open"
            @click="handleOpen"
            >詳情</span
          >
          <span
            class="btn btn-info text-white"
            @click="handleUpdateCenter(store.geometry.coordinates)"
            >地圖</span
          >
        </div>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    store: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      open: false
    }
  },
  methods: {
    handleOpen() {
      this.open = !this.open
    },
    handleUpdateCenter(coordinates) {
      this.$emit('handleUpdateCenter', coordinates)
    }
  }
}
</script>

<style lang="scss" scoped>
.pointer {
  cursor: pointer;
}
</style>
