import panAnJson from "@/assets/json/panan.json";
import sh from './xx.json'
import {getInfoById} from '../api/dp'
export default {
    data() {
        return {
            mapBox: null,
            currentMarkers: [],
            market: null
        }
    },
    mounted() {
    },
    watch: {
        activeTab() {
            this.currentMarkers.forEach(item => {
                item.remove();
            })
            this.mapBox.remove();
            this._mapInit({
                el: "mapBox"
            })
        }
    },
    methods: {
        getInfoById(id, el, status){
            getInfoById({id: id, type: (status == 1 || status == 2) ? 0 : 1}).then(res => {
                let html =  ''
                if(status == 1 || status == 2){
                   html = `<div class="pos" style="
                    width: 384px;
                    background-size: cover;
                    z-index: 1000;
                    padding: 40px 20px 0;
                    box-sizing: border-box;
                    height: 301px;">
                    <img style="position: absolute;top: 14px;right: 21px;width: 80px;height: 80px;" src="${status == 1 ? require('@/assets/image/dsj.png') : require('@/assets/image/sj.png')}" />
                    <p style="font-size: 14px;
                    font-family: Microsoft YaHei;
                    font-weight: 400;
                    color: #EFF7FF;">任务编号：${res.data.orderNumber}</p>
                    <div style="display: flex;
                    justify-content: space-around;
                    font-size: 14px;
                    margin-top: 20px;
                    font-family: Microsoft YaHei;
                    font-weight: 400;
                    color: #EFF7FF;">
                    <p style="width:50%">农户姓名：${res.data.name}</p>
                    <p style="width:50%">联系方式：${res.data.telephoneNumber}</p>
                    </div>
                    <div style="display: flex;
                    justify-content: space-around;
                    font-size: 14px;
                    margin-top: 10px;
                    font-family: Microsoft YaHei;
                    font-weight: 400;
                    color: #EFF7FF;">
                    <p style="width:50%">所属乡镇：${res.data.region}</p>
                    <p style="width:50%">所属村：${res.data.address}</p>
                    </div>
                    <div style="display: flex;
                    justify-content: space-around;
                    font-size: 14px;
                    margin-top: 10px;
                    font-family: Microsoft YaHei;
                    font-weight: 400;
                    color: #EFF7FF;">
                    <p style="width:50%">秸秆类型：${res.data.strawType}</p>
                    <p style="width:50%">直线距离：</p>
                    </div>
                </div>`
                }else if(status == '收储中心'){
                    html = `<div class="pos2" style="
                        width: 245px;
                        background-size: 100% 100%;
                        z-index: 1000;
                        padding: 40px 20px 0;
                        box-sizing: border-box;
                        height: 194px;">
                        <p style="font-size: 14px;
                        font-family: Microsoft YaHei;
                        font-weight: 400;
                        color: #EFF7FF;">收储中心：${res.data.name}</p>
                        <div
                        style="font-size: 14px;
                        margin-top: 10px;
                        font-family: Microsoft YaHei;
                        font-weight: 400;
                        color: #EFF7FF;">
                        <p>所属乡镇：${res.data.region}</p>
                        <p style="marign: 20px 0">所属村：${res.data.address}</p>
                        </div>
                    </div>`
              }else{
                html = `<div class="pos2" style="
                    width: 245px;
                    background-size: 100% 100%;
                    z-index: 1000;
                    padding: 40px 20px 0;
                    box-sizing: border-box;
                    height: 194px;">
                    <p style="font-size: 14px;
                    font-family: Microsoft YaHei;
                    font-weight: 400;
                    color: #EFF7FF;">示范点：${res.data.name}</p>
                    <div
                    style="font-size: 14px;
                    margin-top: 10px;
                    font-family: Microsoft YaHei;
                    font-weight: 400;
                    color: #EFF7FF;">
                    <p>所属乡镇：${res.data.region}</p>
                    <p style="marign: 20px 0">所属村：${res.data.address}</p>
                    </div>
                </div>`
              }
              el.innerHTML = html
            })
        },
        // 打点方法实例marker
        // addPoint() {
        //     let arr = []
        //     let elList = '';
        //     this.mapList.forEach((item, i) => {
        //         let id = ''
        //         const el = document.createElement('div');
        //         let width = 25
        //         let height = 32
        //         let url = ''
        //         if(item.status == 1){
        //             url = require('@/assets/image/point1.png')
        //         }else if(item.status == 2){
        //             url = require('@/assets/image/point2.png')
        //         }else if(item.status == '收储中心'){
        //             url = require('@/assets/image/home.png')
        //             height = 27
        //         }else if(item.status == '示范点'){
        //             url = require('@/assets/image/home2.png')
        //             height = 25
        //         }
        //         el.className = 'marker';
        //         el.style.backgroundImage = `url(${url})`;
        //         el.style.width = `${width}px`;
        //         el.style.height = `${height}px`;
        //         el.style.backgroundSize = '100%';
        //         if(this.activeTab == '收储加工'){
        //             if(item.status == '示范点' || item.status == '收储中心'){
        //                 el.style.display = 'block'
        //             }else{
        //                 el.style.display = 'none'
        //             }
        //         }else{
        //             if(item.status == '示范点'){
        //                 el.style.display = 'none'
        //             }else{
        //                 el.style.display = 'block'
        //             }
        //         }
        //         el.onclick = () => {
        //             if(item.id == id){
        //                 el.style.zIndex = '1'
        //                 el.innerHTML = ''
        //                 id = ''
        //             }else{
        //                 if(elList){
        //                     elList.style.zIndex = '1'
        //                     elList.innerHTML = ''
        //                     id = ''
        //                 }
        //                 elList = el
        //                 el.style.zIndex = '1000'
        //                 id = item.id
        //                 this.getInfoById(item.id, el, item.status)
        //             }
        //         }
        //         //想改变打点样式 请参考http://www.mapbox.cn/mapbox-gl-js/api/#marker
        //         if(item.longitudeLatitude){
        //             this.market = new this.mbgl.Marker(el)
        //             .setLngLat(item.longitudeLatitude.split(','))
        //             .addTo(this.mapBox)
        //             this.currentMarkers.push(this.market)
        //         }
        //     })
        // },
        // 绘制边界线
        drawCityBorder() {
            let that = this
            that.mapBox.on('load', function () {
              that.addPolyline(sh)
            })
          },
        addPolyline(sh) {
            let that = this
            let hoveredStateId = null
            // 加载区域边界geojson数据文件
            that.mapBox.addSource('states', {
              type: 'geojson',
              data: sh,
            })
            // 给区域绘制边框线
            // that.mapBox.addLayer({
            //   id: 'state-borders',
            //   type: 'line',
            //   source: 'states',
            //   layout: {},
            //   paint: {
            //     'line-color': '#627BC1',
            //     'line-width': 2
            //   }
            // })
        },
        _mapInit({
            el = "mapBox",
            zoom = 9.8,
            fillColor = "rgba(0, 0, 0, 0)",
            center = [120.53513, 29.052627],
            drawControl = false
        }) {
            this.mapBox = null;
            window.mapPopup = null;
            this.mbgl.accessToken = 'pk.eyJ1IjoiZWFyc2hlYWx0aCIsImEiOiJjbGFqYnY4ZXUwNDhzM3duaDU5d3IxeG5rIn0.WtRAUAJGxvUYPQ2dRTFhwg'
            this.$nextTick(() => {
                this.mapBox = new this.mbgl.Map({
                    container: el, // container id 
                    // pitch: 50,
                    minZoom: 1.4,
                    // 调整地图的大小
                    maxZoom: 17,
                    style: {
                        version: 8,
                        glyphs: "https://www.zjxxhz.com:9443/iserver/services/map-china400/rest/maps/China/tileFeature/sdffonts/{fontstack}/{range}.pbf",
                        sources: {
                            t_map_1: {
                                type: "raster",
                                tiles: [
                                    "https://t2.tianditu.gov.cn/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=e0980c909c79715b94100ced2f4d955d"
                                ],
                                tileSize: 256
                            },
                            t_map_2: {
                                type: "raster",
                                tiles: [
                                    "https://t2.tianditu.gov.cn/DataServer?T=cia_w&x={x}&y={y}&l={z}&tk=e0980c909c79715b94100ced2f4d955d"
                                ],
                                tileSize: 256
                            },
                            pan_an_json: {
                                type: "geojson",
                                data: panAnJson
                            }
                        },
                        layers: [{
                            id: "t_map_1",
                            type: "raster",
                            source: "t_map_1"
                        },
                        {
                            id: "t_map_2",
                            type: "raster",
                            source: "t_map_2"
                        },
                        {
                            id: "pan_an_json",
                            type: "fill",
                            source: "pan_an_json",
                            paint: {
                                "fill-color": fillColor
                                // 'fill-outline-color': 'rgba(200, 100, 240, 1)'
                            }
                        },
                        {
                            id: "pan_an_json_line",
                            type: "line",
                            source: "pan_an_json",
                            // background: {
                            //   'background-color': '#000000'
                            // },
                            filter: ["==", "$type", "Polygon"],
                            paint: {
                                "line-color": "#26F2FC",
                                "line-width": 0,
                                "line-opacity": 1
                                // 'line-dasharray': [2, 1]
                            }
                        }
                        ]
                    },
                    center: center, // starting position
                    zoom: zoom // starting zoom
                });
                this.drawCityBorder()
                setTimeout(() => {
                    let mbData = [];
                    // 获取json数据格式
                    panAnJson.features[0].geometry.coordinates.map(v => {
                        mbData.push(v);
                    });
                    console.log(mbData ,'1213');
                    panAnJson.features.map((v, i) => {

                        this.mapBox.addLayer({
                            //蒙版边界
                            id: "mb-fill2" + i + new Date().getTime(),
                            type: "fill",
                            source: {
                                type: "geojson",
                                data: {
                                    type: "FeatureCollection",
                                    features: [v]
                                }
                            },
                            paint: {
                                "fill-color": `#f1d04d`,
                                "fill-opacity": 0,
                            },
                            layout: {
                                visibility: "visible"
                            }
                        });
                        this.mapBox.addLayer({
                            //    添加地图边界
                            id: "mb-line2" + i + new Date().getTime(),
                            type: "line",
                            source: {
                                type: "geojson",
                                data: {
                                    type: "FeatureCollection",
                                    features: [v]
                                }
                            },
                            paint: {
                                "line-color": `#e6e6e4`,
                                "line-width": 6
                            },
                            layout: {
                                visibility: "visible"
                            }
                        });
                    })
                    // 添加除了地图意外的蒙板
                    this.mapBox.addLayer({
                        id: "mb-tag",
                        type: "fill",
                        source: {
                            type: "geojson",
                            data: {
                                type: "Feature",
                                geometry: {
                                    type: "Polygon",
                                    coordinates: [
                                        [
                                            [-180, 90],
                                            [180, 90],
                                            [180, -90],
                                            [-180, -90]
                                        ],
                                        ...mbData
                                    ]
                                }
                            }
                        },
                        paint: {
                            // 调整蒙版的透明度
                            "fill-color": "rgba(27, 37, 58,.85)"
                            // 'fill-opacity': 1 /* 透明度 */,
                        },
                        layout: {
                            visibility: "visible"
                        }
                    });
                    this.addPoint()
                }, 100);
            })    
        },
    }
}