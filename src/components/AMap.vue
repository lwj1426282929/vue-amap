<template>
  <div id="container"></div>
</template>

<script>
import AMap from 'AMap'
// import Loca from 'Loca'

export default {
  name: 'AMap',

  data () {
    return {
      map: '',
      markers: [
        {
          lng: 105.005,
          lat: 32.349,
          name: '1'
        },
        {
          lng: 104.602,
          lat: 32.067,
          name: '2'
        },
        {
          lng: 105.272,
          lat: 32.451,
          name: '3'
        },
        {
          lng: 105.042,
          lat: 32.402,
          name: '4'
        },
        {
          lng: 104.982,
          lat: 32.24,
          name: '5'
        },
      ]
    }
  },

  mounted () {
    this.map = new AMap.Map( 'container', {
      mapStyle: 'amap://styles/dark',
      features: ['bg', 'point'],
      center: [105.005, 32.349,]
    } )

    this.renderMarkers()
  },

  methods: {
    // 渲染点
    renderMarkers () {
      this.markers.forEach( item => {
        let circleMarker = new AMap.CircleMarker( {
          center: [item.lng, item.lat],
          radius: 10,
          strokeColor: 'white',
          strokeWeight: 2,
          strokeOpacity: 0.5,
          fillColor: 'red',
          fillOpacity: 0.5,
          zIndex: 10,
          bubble: true,
          cursor: 'pointer',
          clickable: true,
          extData: { ...item }
        } )
        circleMarker.on( 'click', ( e ) => {
          // console.log( e.target.getExtData() )
          let target = e.target.getExtData()
          let rectangle = new AMap.Rectangle({
            map: this.map,
            bounds: new AMap.Bounds(new AMap.LngLat(target.lng - 10, target.lat - 10), new AMap.LngLat(target.lng + 10, target.lat + 10)),
            extData: target
          })
          rectangle.setMap(this.map)
        } )
        circleMarker.setMap( this.map )
      } )
    }
  }
}
</script>

<style lang="less" scoped>
#container {
  width: 100%;
  height: 600px;
}
</style>