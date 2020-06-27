<template>
  <div class="home">
    <top-view />
    <sales-view />
    <bottom-view />
    <map-view />
  </div>
</template>

<script>
import TopView from './components/TopView'
import SalesView from './components/SalesView'
import MapView from './components/MapView'
import BottomView from './components/BottomView'
import { wordcloud, mapScatter, screenData } from '@/api/home'

export default {
  name: 'Home',

  components: {
    TopView,
    SalesView,
    MapView,
    BottomView
  },

  provide() {
    return {
      getReportData: this.getReportData,
      getWordCloud: this.getWordCloud,
      getMapData: this.getMapData
    }
  },

  data() {
    return {
      reportData: null,
      wordCloud: null,
      mapData: null
    }
  },

  mounted() {
    wordcloud().then(data => {
      this.wordCloud = data
    })
    mapScatter().then(data => {
      this.mapData = data
    })
    screenData().then(data => {
      this.reportData = data
    })
  },

  methods: {
    getReportData() {
      return this.reportData
    },

    getWordCloud() {
      return this.wordCloud
    },

    getMapData() {
      return this.mapData
    }
  }
}
</script>

<style>
.home {
  min-height: 100%;
  width: 100%;
  padding: 20px;
  background: #eee;
  box-sizing: border-box;
}
</style>
