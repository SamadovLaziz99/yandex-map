<template>
  <div title="yandex-map" style="width: 100%">
    <div id="map" ref="basicMap" class="yandexMap"></div>
  </div>
</template>

<script>
import ymaps from 'ymaps';
import {defaultMap} from "@/constants/config";

export default {
  name: "yandex-map",
  data() {
    return {
      map: null,
      maps: null,
      coords: [-34.397, 150.644],
      center: {lat: -34.397, lng: 150.644},
      markers: [
        {
          position: {lat: -34.397, lng: 150.644}
        }
      ]
    }
  },
  methods: {
    initMap() {
      ymaps.load(defaultMap.api).then(maps => {
        console.log("MAPS: ", maps);
        this.maps = maps;
        this.map = new maps.Map('map', {
          center: defaultMap.home,
          zoom: 15,
          controls: ['zoomControl', 'fullscreenControl']
        });
        // console.log(this.map, this.maps);
        // this.map.events.add('click', (e) => this.clickedMap(e));
        // this.map.container.fitToViewport();
      })
          .catch(error => console.log('Failed to load Yandex Maps', error));
    },
  },
  created() {
    this.initMap()
  },
}
</script>

<style scoped>
.yandexMap {
  height: 100vh;
  width: 100vw;
}
</style>