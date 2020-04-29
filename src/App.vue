<template>
  <div>
    <Header />
    <div class="row ml-0 mr-0">
      <SideBar
        :maskData="maskData"
        :city="city"
        :town="town"
        :pages="pages"
        :currentPage="currentPage"
        :currentCity.sync="currentCity"
        @handleTownSelect="handleTownSelect"
        @handleUpdateCenter="handleUpdateCenter"
        @handlePageChange="handlePageChange"
        :byAdult.sync="byAdult"
        :byChild.sync="byChild"
      />
      <div class="col-lg-8 col-xl-9 map pl-0 pr-0">
        <Map :maskData="result" :manualCenter="manualCenter" />
      </div>
    </div>
    <Spinner v-show="loading" />
  </div>
</template>

<script>
import Header from '@/components/Header'
import SideBar from '@/components/SideBar'
import Map from '@/components/Map'
import Spinner from '@/components/Spinner'
export default {
  name: 'App',
  components: {
    Header,
    SideBar,
    Map,
    Spinner
  },
  data() {
    return {
      data: [],
      currentCity: '',
      currentTown: '',
      city: [],
      result: [],
      maskData: [],
      manualCenter: [],
      pages: 0,
      limit: 3,
      currentPage: 0,
      byAdult: false,
      byChild: false,
      loading: false
    }
  },
  computed: {
    town: {
      get() {
        const target = this.data.find(
          item => item.CityName === this.currentCity
        )
        return target
          ? target.AreaList.map(town => {
            return {
              name: town.AreaName,
              value: town.AreaName,
              id: town.ZipCode
            }
          })
          : []
      }
    },
    startPage() {
      return this.currentPage * this.limit
    },
    endPage() {
      return this.startPage + this.limit
    }
  },
  watch: {
    byAdult: {
      handler() {
        this.resetPageAndFetch()
      }
    },
    byChild: {
      handler() {
        this.resetPageAndFetch()
      }
    }
  },
  methods: {
    fetchCityData() {
      this.axios.get('/cityCountyData.json').then(res => {
        this.data = res.data
        this.city = res.data.map((item, index) => {
          return {
            name: item.CityName,
            value: item.CityName,
            id: index
          }
        })
      })
    },
    fetchMaskData(start, end) {
      this.loading = true
      return this.axios
        .get(
          'https://raw.githubusercontent.com/kiang/pharmacies/master/json/points.json'
        )
        .then(res => {
          const data = res.data
          this.result = data.features.filter(
            item =>
              item.properties.town === this.currentTown &&
              item.properties.county === this.currentCity
          )
          this.byAdult &&
            this.result
              .sort((a, b) => a.properties.mask_adult - b.properties.mask_adult)
              .reverse()
          this.byChild &&
            this.result
              .sort((a, b) => a.properties.mask_child - b.properties.mask_child)
              .reverse()
          this.maskData = this.result.slice(start, end)
          this.pages = Math.ceil(this.result.length / this.limit)
          this.loading = false
        })
    },
    resetPageAndFetch() {
      this.currentPage = 0
      this.fetchMaskData(this.startPage, this.endPage)
    },
    handleTownSelect(town) {
      this.currentTown = town
      this.resetPageAndFetch()
    },
    handlePageChange(val) {
      if (val === 'inc') {
        if (this.currentPage + 1 !== this.pages) {
          this.currentPage += 1
        }
      } else if (val === 'dec') {
        if (this.currentPage > 0) {
          this.currentPage -= 1
        }
      } else {
        this.currentPage = parseInt(val)
      }
      this.fetchMaskData(this.startPage, this.endPage)
    },
    handleUpdateCenter(coordinates) {
      this.manualCenter = coordinates
    }
  },
  created() {
    this.fetchCityData()
  }
}
</script>

<style lang="scss">
@import '../node_modules/bootstrap/scss/functions';
@import './assets/variables';
@import '../node_modules/bootstrap/scss/mixins';

@import '../node_modules/bootstrap/scss/bootstrap';
@import url('https://fonts.googleapis.com/css?family=Noto+Sans+TC:100,400,700&display=swap');

body {
  font-family: 'noto sans tc', sans-serif;
  overflow-x: hidden;
}
.map {
  height: calc(100vh - 58px);
  position: relative;
  @media (max-width: 768px) {
    height: 50vh;
  }
}
</style>
