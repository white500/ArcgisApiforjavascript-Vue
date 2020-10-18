<!--
 * @Author: your name
 * @Date: 2020-10-18 21:31:53
 * @LastEditTime: 2020-10-18 23:44:30
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \arcgis-test-demo\src\views\Home.vue
-->
<template>
  <div id="mapView"></div>
</template>

<script>
import { loadModules } from 'esri-loader'
export default {
  name: 'map-view',
  data () {
    return {
      view: null
    }
  },
  components: {},
  mounted () {
    this.creatMap()
  },
  methods: {
    creatMap () {
      loadModules([
        "esri/Map",
        "esri/views/MapView",
        "esri/Basemap",
        "esri/layers/WebTileLayer"
      ], { css: true })
      .then(([
        Map,
        MapView,
        Basemap,
        WebTileLayer,
      ]) => {
        const wtl = new WebTileLayer({
          urlTemplate: 'http://mt0.google.cn/vt/lyrs=t@132,r@280000000&hl=zh-CN&gl=cn&src=app&x={col}&y={row}&z={level}&s=Gali'
        });
        var basemap = new Basemap({
          baseLayers: [
           wtl
          ],
          referenceLayers: [
            wtl
          ],
        });

        // add the new instance of the custom tile layer the map
        var map = new Map({
          basemap: basemap
        });
        
        this.view = new MapView({
          container: this.$el,
          map: map,
          center: [116.403119, 39.914935],
          zoom: 12
        })
      })
    }
  },
  destroyed () {
    if (this.view) {
      // destroy the map view
      this.view.destroyed()
    }
  }
}
</script>
<style>
#mapView {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 80vh;
  border: 1px solid black;
}
</style>
