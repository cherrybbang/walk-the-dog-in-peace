<template>
  <p>Walk Your Dog in Peace!</p>

  <div>
    <div id="map" style="width: 100%; height: 500px"></div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue'

const { VITE_KAKAO_MAP_KEY } = import.meta.env

const loadKakaoMap = () => {
  const script = document.createElement('script')
  script.src = `//dapi.kakao.com/v2/maps/sdk.js?appkey=${VITE_KAKAO_MAP_KEY}&autoload=false`
  script.onload = () => {
    window.kakao.maps.load(() => {
      getMap()
    })
  }
  document.head.appendChild(script)
}

const getMap = () => {
  const container = document.getElementById('map')

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(async (position) => {
      const lat = position.coords.latitude
      const lon = position.coords.longitude
      console.log(lat, lon)

      const options = {
        center: new window.kakao.maps.LatLng(lat, lon),
        level: 5,
      }

      // new window.kakao.maps.Map(container, options)

      // 마커생성
      const map = new window.kakao.maps.Map(container, options)
      const marker = new window.kakao.maps.Marker({
        position: map.getCenter(),
      })
      marker.setMap(map)
    })
  }
  console.log('Map is ready!')
}

onMounted(() => {
  loadKakaoMap()
})
</script>
