<template>
  <div class="col-lg-4 col-xl-3 sidebar pl-5 pr-5">
    <div class="form-group row mt-3">
      <label for="city" class="col-3 col-form-label">城市</label>
      <select
        name=""
        id="city"
        class="form-control col-7"
        @change="handleCitySelect($event)"
      >
        <option value="">請選擇城市</option>
        <option :value="item.value" v-for="item in city" :key="item.id">{{
          item.name
        }}</option>
      </select>
    </div>
    <div class="form-group row">
      <label for="city" class="col-3 col-form-label" v-show="currentCity !== ''"
        >地區</label
      >
      <select
        name=""
        id="city"
        class="form-control col-7"
        v-show="currentCity !== ''"
        @change="handleTownSelect($event)"
      >
        <option value="">請選擇地區</option>
        <option :value="item.value" v-for="item in town" :key="item.id">{{
          item.name
        }}</option>
      </select>
    </div>
    <div
      class="pt-3 pb-3 d-flex align-items-end flex-wrap"
      v-if="currentCity.length > 0 && maskData.length > 0"
    >
      <h4>搜尋結果</h4>
      <select
        name=""
        id=""
        class="ml-auto form-control"
        style="width: 150px"
        @change="handleSort($event)"
      >
        <option value="">排序</option>
        <option value="adult">成人口罩</option>
        <option value="child">兒童口罩</option>
      </select>
    </div>

    <ul class="list-group" v-if="currentCity.length > 0 && maskData.length > 0">
      <PharmacyItem
        v-for="store in maskData"
        :key="store.properties.name"
        :store="store"
        @handleUpdateCenter="handleUpdateCenter"
      />
    </ul>
    <div
      class="d-flex justify-content-center"
      v-if="currentCity.length > 0 && maskData.length > 0"
    >
      <Pagination
        :pages="pages"
        @handlePageChange="handlePageChange"
        :currentPage="currentPage"
      />
    </div>
  </div>
</template>

<script>
import PharmacyItem from '@/components/PharmacyItem'
import Pagination from '@/components/Pagination'
export default {
  components: {
    PharmacyItem,
    Pagination
  },
  props: {
    byAdult: {
      type: Boolean,
      required: true
    },
    byChild: {
      type: Boolean,
      required: true
    },
    currentCity: {
      type: String,
      required: true
    },
    city: {
      type: Array,
      required: true
    },
    town: {
      type: Array,
      required: true
    },
    maskData: {
      type: Array,
      required: true
    },
    pages: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    }
  },
  methods: {
    handleCitySelect(e) {
      this.$emit('update:currentCity', e.target.value)
    },
    handleTownSelect(e) {
      this.$emit('handleTownSelect', e.target.value)
    },
    handleUpdateCenter(coordinates) {
      this.$emit('handleUpdateCenter', coordinates)
    },
    handlePageChange(val) {
      this.$emit('handlePageChange', val)
    },
    handleSort(e) {
      const data = e.target.value
      if (data === 'adult') {
        this.$emit('update:byAdult', true)
        this.$emit('update:byChild', false)
      } else if (data === 'child') {
        this.$emit('update:byChild', true)
        this.$emit('update:byAdult', false)
      } else {
        this.$emit('update:byChild', false)
        this.$emit('update:byAdult', false)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.sidebar {
  background-color: #eee;
}
</style>
