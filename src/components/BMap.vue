<template>
    <div id="chart"></div>
</template>

<script>
import AMap from 'AMap'

export default {
    name: 'BMap',

    data () {
        return {
            map: '',
            // 标记点
            markers: [],
            // 信息窗体
            infoWindows: [],
            // 连接线
            polylines: []
        }
    },

    mounted () {
        this.map = new AMap.Map('chart', {
            features: ['bg', 'point'],
            zoomEnable: false
        })

        // this.renderMarkers()
    },

    methods: {
        renderMarkers () {
            let res = [
                { lng: 121.458421, lat: 31.27821, name: '上海易居', address: '上海市静安区xxx', tel: 'xxx-xxxx-xxxx' },
                { lng: 121.499717, lat: 31.239702, name: '东方明珠塔', address: 'xxxxxxx', tel: 'xxx-xxxx-xxxx' },
                { lng: 121.44532, lat: 31.223505, name: '静安寺', address: 'xxxxxxxxx', tel: 'xxx-xxxx-xxxx' },
                { lng: 121.339785, lat: 31.196056, name: '虹桥国际机场', address: '虹桥国际机场', tel: 'xxx-xxxx-xxxx' },
                { lng: 121.808682, lat: 31.142267, name: '浦东国际机场', address: '浦东国际机场', tel: 'xxxxxxxx' },
            ]

            res.forEach((item, index) => {
                let circleMarker = new AMap.CircleMarker({
                    center: [item.lng, item.lat],
                    radius: 10,
                    strokeColor: '#FF0000',
                    strokeOpacity: 1,
                    fillColor: '#FF0000',
                    fillOpacity: 1,
                    cursor: 'pointer',
                    clickable: true
                })

                let infoWindow = new AMap.Marker({
                    position: new AMap.LngLat(item.lng, item.lat),
                    draggable: true,
                    offset: new AMap.Pixel(100, -152),
                    visible: false,
                    topWhenClick: true,
                    content: `<div class="info-window">
                                  <div class="info-window-header">
                                    <span>${item.name}</span>  
                                    <span class="close" data-index="${index}">+</span>
                                  </div>
                                  <div class="info-window-body">
                                    <p class="info-window-address">
                                        <span class="label">地址：</span>
                                        <span class="content">${item.address}</span>
                                    </p>
                                    <p class="info-window-address">
                                        <span class="label">电话：</span>
                                        <span class="content">${item.tel}</span>
                                    </p>
                                  </div>
                              </div>`
                })

                let position = this.map.lngLatToContainer(new AMap.LngLat(item.lng, item.lat))
                let endPoint = this.map.containerToLngLat(new AMap.Pixel(position.x + 100, position.y - 76))

                let polyline = new AMap.Polyline({
                    path: [[item.lng, item.lat], [item.lng, endPoint.lat], [endPoint.lng, endPoint.lat]],
                    strokeColor: '#FF0000',
                    strokeStyle: 'dashed',
                    strokeWeight: 1,
                    draggable: true
                })
                polyline.hide()

                // 显示信息弹窗与连接线
                circleMarker.on('click', () => {
                    infoWindow.show()
                    polyline.show()
                })

                // 信息弹窗拖拽
                infoWindow.on('dragging', () => {
                    // 转换成像素
                    let endPixel = this.map.lngLatToContainer(new AMap.LngLat(infoWindow.getPosition().lng, infoWindow.getPosition().lat))
                    // 处理像素差后在转换回坐标
                    let endPoint = this.map.containerToLngLat(new AMap.Pixel(endPixel.x + 100, endPixel.y - 76))
                    polyline.setPath([[item.lng, item.lat], [item.lng, endPoint.lat], [endPoint.lng, endPoint.lat]])
                })

                this.markers.push(circleMarker)
                this.infoWindows.push(infoWindow)
                this.polylines.push(polyline)
            })

            // 将点、窗体信息、连接线添加到图层
            this.map.add(this.markers)
            this.map.add(this.infoWindows)
            this.map.add(this.polylines)
        }
    }
}
</script>

<style lang="less" scoped>
#chart {
    width: 100%;
    height: 800px;
    position: relative;

    /deep/.info-window {
        width: 300px;
        min-height: 150px;
        background: #fff;
        border: 1px solid #000;
        border-radius: 8px;
        z-index: 999;
        cursor: move;
        overflow: hidden;
        box-sizing: border-box;

        .info-window-header {
            height: 30px;
            line-height: 30px;
            color: #fff;
            background: #333;
            padding: 0 5px 0 10px;
            display: flex;
            align-items: center;
            justify-content: space-between;

            .close {
                display: block;
                font-size: 30px;
                cursor: pointer;
                text-align: center;
                transform: rotate(45deg);

                &:hover {
                    color: #409eff;
                }
            }
        }

        .info-window-body {
            padding: 10px;

            p {
                padding: 0;
                margin: 0;
                display: flex;
                line-height: 1.5em;

                .label {
                    width: 50px;
                }

                .content {
                    flex: 1;
                }
            }
        }
    }
}
</style>