<template>
    <div id="map" class="map"></div>
</template>

<script>
    import L from "leaflet";

    export default {
        name: "Map",
        data() {
            return {
                zoom: 6,
                map: null,
                tileLayer: null,
                layers: [],
                icon: L.divIcon({
                    className: 'leaflet-custom-marker',
                    iconSize: new L.Point(36, 36),
                    iconAnchor: [17, 34]
                })
            };
        },
        props: {
            location: Object,
            changeLocation: Function
        },
        methods: {
            initMap() {
                this.map = L.map('map').setView([this.location.lat, this.location.long], this.zoom);
                this.tileLayer = L.tileLayer(
                    'https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png',
                    {
                        maxZoom: 18,
                        attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, &copy; <a href="https://carto.com/attribution">CARTO</a>',
                    }
                );
                this.tileLayer.addTo(this.map);
                this.initMarker(this.location.lat, this.location.long);

                this.map.on('click', this.moveMarker);

            },
            updateLocation() {
                let latlng = this.marker.getLatLng();
                this.changeLocation({lat: latlng.lat, long: latlng.lng})
            },
            moveMarker(e) {
                let latlng = e.latlng;
                if (this.marker) {
                    this.map.removeLayer(this.marker);
                }
                this.initMarker(latlng.lat, latlng.lng);
            },
            initMarker(lat, long) {
                this.marker = L.marker([lat, long], {title: "Location", draggable: true, icon: this.icon})
                    .addTo(this.map);
                this.marker.on('dragend', this.updateLocation);
                this.updateLocation()
            }
        },
        mounted() {
            this.initMap();
        },
        watch: {
            location: {
                handler() {
                    this.changeLocation(this.location);
                    this.map.setView([this.location.lat, this.location.long]);
                    let latlng = this.marker.getLatLng();
                    if(latlng.lat !== this.location.lat || latlng.lng !== this.location.long){
                        this.moveMarker({latlng: {lat: this.location.lat, lng: this.location.long}})
                    }
                },
                deep: true
            }
        }
    };
</script>

<style>
    @import "https://unpkg.com/leaflet@1.2.0/dist/leaflet.css";

    .map {
        height: 500px;
    }

    .leaflet-custom-marker {
        background: transparent;
        background-image: url("../assets/baseline_room_black_18dp.png");
    }
</style>