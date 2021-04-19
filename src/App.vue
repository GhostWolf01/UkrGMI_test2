<template>
  <div id="app">
    <div id="map" ></div>
    <div class="map__block">
      <label class="map__text">{{clickMap}}</label>
    </div>
  </div>
</template>

<script>
import {Loader} from 'google-maps';
import markerStyle from './assets/map/marker.png';

export default {
  name: 'App',
  data() {
    return {
      clickMap: '',
      map: null,
      markers: [],
      mapCenter: { lat: 49.0515383513631, lng: 30.94063667129703 },
      mapLatlngs: [
        {
          name: "Київ",
          mapLatlng: { lat: 50.450063458398375, lng: 30.523520894094492 }
        },
        {
          name: "Львів",
          mapLatlng: { lat: 49.83850857421318, lng: 24.01832406489726 }
        },
        {
          name: "Дніпро",
          mapLatlng: { lat: 48.466525104518404, lng: 35.04556418316153 }
        },
        {
          name: "Донецьк",
          mapLatlng: { lat: 48.00973471001179, lng: 37.8158220297957 }
        },
        {
          name: "Одеса",
          mapLatlng: { lat: 46.47610923862431, lng: 30.715355298029465 }
        },
        {
          name: 'Житомир',
          mapLatlng: { lat: 50.253027436775206, lng: 28.644385857929183 }
        }
      ]
    }
  },
  mounted() {
    let apiKey = "AIzaSyCuJQvEPFtEnnUeDksw0T1BFBW0KNL3DHM";
    let loaderOptions = {
      language: 'ua', 
      libraries: ['geometry', 'places', 'visualization']
    };
    let loader = new Loader( apiKey, loaderOptions);
    let options = {
          zoom: 5,
          center: this.mapCenter,
          mapTypeId: 'roadmap',
        }
    loader.load().then((google) => {
      this.map = new google.maps.Map(document.getElementById('map'), options);
      for (const Latlng of this.mapLatlngs) {
        let marker = new google.maps.Marker({
          position: Latlng.mapLatlng,
          map: this.map,
          title:`${Latlng.name} : lat: ${Latlng.mapLatlng.lat} lng: ${Latlng.mapLatlng.lng}`,
          icon: markerStyle,
        })
        google.maps.event.addListener(marker, 'click', ()=>{
          this.clickMap = marker.getTitle();
        })
        this.markers.push(marker);
      }
    })
  }
}
</script>

<style lang="scss">
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app{
  margin: auto;
  width: 600px;
  height: 400px;
}

#map{
  width: 100%;
  height: 100%;
}

.map{
  &__block{
    margin-top: 20px;
  }
  &__text{
    font-family: 'Roboto';
    font-weight: 400;
    font-size: 28px;
    color: black;
    line-height: 28px;
    text-align: center;
  }
}
</style>
