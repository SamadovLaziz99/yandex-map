<template>
  <div title="yandex-map" style="width: 100%">
    <div id="map" ref="basicMap" class="yandexMap"></div>
  </div>
</template>

<script>
import ymaps from 'ymaps';
import {defaultMap} from "@/constants/config";
import pin from "@/assets/pin-icon.png";

export default {
  name: "yandex-map",
  data() {
    return {
      map: null,
      icon: null,
      pin: pin,
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
        // console.log("MAPS: ", maps);
        this.maps = maps;
        this.map = new maps.Map('map', {
          center: defaultMap.home,
          zoom: 13,
          controls: ['zoomControl', 'fullscreenControl']
        }, {
          searchControlProvider: 'yandex#search'
        });

        const myPlacemark = new this.maps.Placemark([41.312947, 69.280204], {
          // In order for the balloon and hint to open on the placemark, you need to set certain properties.
          balloonContentHeader: "The placemark balloon",
          balloonContentBody: "Content of the <em>placemark</em> balloon",
          balloonContentFooter: "Basement",
          hintContent: "The placemark hint"
        });

        const myGeoObject = new this.maps.GeoObject({
          // The geometry description.
          geometry: {
            type: "Point",
            coordinates: [41.3, 69.28]
          },
          properties: {
            iconContent: "Sudralib yuradigan",
            hintContent: "Suring!"
          }
        }, {
          preset: "islands#blackStretchyIcon",
          // Agar draggable: false bo'lsa sudralmaydi
          draggable: true
        });

        const myPieChart = new this.maps.Placemark([
          41.32, 69.25
        ], {
          // Data for generating a diagram.
          data: [
            {weight: 8, color: '#0E4779'},
            {weight: 6, color: '#1E98FF'},
            {weight: 4, color: '#82CDFF'}
          ],
          iconCaption: "Diagram"
        }, {
          // Defining a custom placemark layout.
          iconLayout: 'default#pieChart',
          // Radius of the diagram, in pixels.
          iconPieChartRadius: 30,
          // The radius of the central part of the layout.
          iconPieChartCoreRadius: 10,
          // Fill style for the core.
          iconPieChartCoreFillStyle: '#ffffff',
          // The style for lines between sectors and the outline of the diagram.
          iconPieChartStrokeStyle: '#ffffff',
          // Width of the sector dividing lines and diagram outline.
          iconPieChartStrokeWidth: 3,
          // Maximum width of the placemark caption.
          iconPieChartCaptionMaxWidth: 200
        });

        const Placemark = new this.maps.Placemark([41.311, 69.304], {
          hintContent: 'A custom placemark icon',
          balloonContent: 'This is a pretty placemark'
        }, {
          iconLayout: 'default#image',
          // Custom image for the placemark icon.
          iconImageHref: pin,
          // The size of the placemark.
          iconImageSize: [30, 42],
          iconImageOffset: [-5, -38]
        });

        const myGeoPolyline = new this.maps.GeoObject({
          // Describing the geometry of the geo object.
          geometry: {
            // The "Polyline" geometry type.
            type: "LineString",
            coordinates: [
              // [55.80, 37.50],
              // [55.80, 37.40],
              // [55.70, 37.50],
              // [55.70, 37.40],
              [41.31, 69.3],
              [41.31, 69.29],
              [41.29, 69.3],
              [41.29, 69.29]
            ]
          },
          properties:{
            // The contents of the hint.
            hintContent: "I'm a geo object",
            // The contents of the balloon.
            balloonContent: "You can drag me"
          }
        }, {
          draggable: true,
          // The line color.
          strokeColor: "#000000",
          // Line width.
          strokeWidth: 5
        });

        const myGeoSquare = new this.maps.Rectangle([
          [41.32, 69.31],
          [41.33, 69.32]
        ], {
          //Properties
          hintContent: 'Harakatlanmaydi!',
          balloonContent: 'Kvadrat'
        }, {
          fillColor: '#7df9ff33',
          fillOpacity: 0.5,
          // Stroke color.
          strokeColor: '#0000FF',
          // Stroke transparency.
          strokeOpacity: 0.5,
          // Line width.
          strokeWidth: 2,
          borderRadius: 6
        });

        const myGeoSquareMove = new this.maps.GeoObject({
          // Move
          geometry: {
            type: 'Rectangle',
            coordinates: [
              [41.332, 69.31],
              [41.34, 69.32]
            ]
          },
          properties: {
            hintContent: 'Harakatlanadi',
            balloonContent: 'Harakatlanikidan kvadrat'
          }
        }, {
          draggable: true,
          // The fill color and transparency.
          fillColor: '#ffff0022',
          // The color and transparency of borders.
          strokeColor: '#3caa3c88',
          // Line width.
          strokeWidth: 7
        });

        this.map.geoObjects.add(myPlacemark);
        // https://yandex.com/dev/maps/jsbox/2.1/balloon_and_hint

        this.map.geoObjects.add(myGeoObject);
        // https://yandex.com/dev/maps/jsbox/2.1/balloon_ajax

        this.map.geoObjects.add(myPieChart);
        // https://yandex.com/dev/maps/jsbox/2.1/placemark

        this.map.geoObjects.add(Placemark);
        // https://yandex.com/dev/maps/jsbox/2.1/icon_customImage

        this.map.geoObjects.add(myGeoPolyline);
        // https://yandex.com/dev/maps/jsbox/2.1/polyline

        this.map.geoObjects.add(myGeoSquare);
        // https://yandex.com/dev/maps/jsbox/2.1/rectangle

        this.map.geoObjects.add(myGeoSquareMove);
        // https://yandex.com/dev/maps/jsbox/2.1/rectangle

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