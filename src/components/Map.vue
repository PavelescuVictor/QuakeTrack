<template>
    <div id="map">
        <MglMap :accessToken="accessToken" :mapStyle="mapStyle" :center="coordinates" :zoom="zoom" :attributionControl="false">
            <MglNavigationControl position="top-left" />
            <MglGeolocateControl position="top-left" />
            <MglFullscreenControl position="top-left" />
            <MglGeojsonLayer
                :sourceId="geoJsonSource.data.id"
                :source="geoJsonSource"
                layerId="mylayer"
                :layer="geoJsonLayer"
            />
        </MglMap>
        <div class="map__content">
            <h1>{{ msg }}</h1>
            <div class="form__wrapper">
                <form @submit.prevent="generateQuakesData">
                    <p v-text="formError"></p>
                    <div class="days">
                        <label>Enter number of days</label>
                        <input type="number" id="days" name="days" v-model="days" max="30"/>
                    </div>
                    <div class="depth-max">
                        <label>Maximum depth</label>
                        <input type="number" id="maxDepth" name="maxDepth" v-model="maxDepth" />
                    </div>
                    <div class="depth-min">
                        <label>Minimum depth</label>
                        <input type="number" id="minDepth" name="minDepth" v-model="minDepth" />
                    </div>
                    <div class="magnitude-min">
                        <label for="magnitude">Minimum magnitude</label>
                        <input type="number" id="quantity" name="quantity" v-model="minMagnitude" />
                    </div>
                    <div class="submit">
                        <input type="submit" id="submit" value="Submit" />
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
<script>
import Mapbox from "mapbox-gl";
import { 
    MglMap,
    MglNavigationControl,
    MglGeolocateControl,
    MglFullscreenControl,
    MglGeojsonLayer,
} from "vue-mapbox";
// So that markes dont move weirdly on zoom
import 'mapbox-gl/dist/mapbox-gl.css';
import axios from 'axios';
export default {
    name: "Map",
    components: {
        MglMap, 
        MglNavigationControl,
        MglGeolocateControl,
        MglFullscreenControl,
        MglGeojsonLayer,
    },
    data () {
        return {
            msg: "Earthquakes Map Visualization",
            accessToken: 'pk.eyJ1IjoiMjA0M253IiwiYSI6ImNrZG9qeWhtMDBoMGYycXFvNm9nZnh4YzMifQ.djEvGtmsohtkGeFhV-0OFg',
            mapStyle: 'mapbox://styles/mapbox/streets-v11',
            coordinates: [0, 0],
            zoom: 0,
            days: '',
            maxDepth: '',
            minDepth: '',
            minMagnitude: '',
            formError: '',
            geoJsonSource: {
                type: 'geojson',
                data: {
                    id: "thisIsMySource",
                    type: "FeatureCollection",
                    features: []
                },
            },
            geoJsonLayer: {
                type: "circle",
                paint: {
                    "circle-color": "#4C84FF",
                    "circle-opacity": 0.5,
                    'circle-radius': {
                        'base': 10,
                        'stops': [
                        [12, 10],
                        [22, 180]
                        ]
                    },
                }
            }
        }
    },
    methods: {
        generateQuakesData(){
            this.formError = '';
            let daysSetFlag = true;
            let maxDepthSetFlag = true;
            let minDepthSetFlag = true;
            let minMagnitudeSetFlag = true;

            if(this.days === '') daysSetFlag = false;
            if(this.maxDepth === '') maxDepthSetFlag = false;
            if(this.minDepth === '') minDepthSetFlag = false;
            if(this.minMagnitude === '') minMagnitudeSetFlag = false;
            
            if(daysSetFlag === false) this.formError = "The day counter must be set!";
            else if(maxDepthSetFlag === false) this.formError = "The max depth counter must be set!";
            else if(minDepthSetFlag === false) this.formError = "The min depth counter must be set!";
            else if(minMagnitudeSetFlag === false) this.formError = "The min magnitude counter must be set";
            else{
                var currentdate = new Date();
                var queryDate = new Date();
                queryDate.setDate(currentdate.getDate() - this.days);
                var dateTime = `${currentdate.getFullYear()}-${(currentdate.getMonth()+1)}-${currentdate.getDate()}T${currentdate.getHours()}:${currentdate.getMinutes()}:${currentdate.getSeconds()}`
                var queryDateTime = `${queryDate.getFullYear()}-${(queryDate.getMonth()+1)}-${queryDate.getDate()}T${queryDate.getHours()}:${queryDate.getMinutes()}:${queryDate.getSeconds()}`
                console.log(dateTime);
                console.log(queryDateTime);
                axios.get(`https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime=${queryDateTime}&endtime=${dateTime}&minmagnitude=${this.minMagnitude}&maxdepth=${this.maxDepth}&mindepth=${this.minDepth}`).then(response => {
                this.geoJsonSource = response;
                this.geoJsonSource.data['id'] = "thisIsMySource";
               });
            }
        },
    },
    created(){
        this.mapbox = Mapbox;
    },
   watch: {
       geoJsonSource: function (val){
           console.log(val);
       }
   }
}
</script>
<style scoped>
    #map {
        height: 100%;
        display: grid;
        grid-template-columns: repeat(2, 1fr);
    }

    .map__content{
        display: grid;
        grid-template-rows: auto 1fr;
    }

    .map__content h1 {
        padding: calc(var(--padding-2) / 2);
        font-size: 2rem;
    }

    .form__wrapper {
        display: flex;
        flex-direction: column;
    }

    .form__wrapper p{
        height: var(--padding-1);
        transform: translateX(-50%);
        position: absolute;
        top: 0px;
        line-height: var(--padding-1);
        font-size: 1.4rem;
        background: var(--color-blue);
        padding: 0px var(--padding-1);
        color: #FFFFFF;
        border-bottom-left-radius: 50px;
        border-bottom-right-radius: 50px;
    }
    
    .days, .magnitude-min, .depth-min, .depth-max {
        padding: calc(var(--padding-2) / 2);
        display: grid;
        align-items: left;
    }

    .days label, .magnitude-min label, .depth-min label, .depth-max label {
        line-height: 1.3rem;
        font-size: 1.3rem;
        padding: 10px;
        margin: calc(var(--padding-2) / 4);
    }

    .days input, .magnitude-min input, .depth-min input, .depth-max input {
        border:none;
        outline:none;
        appearance: none;
        height: calc(1.5 * var(--padding-2));
        border-radius:22px;
        box-sizing:border-box;
        padding: 0 var(--padding-2);
        margin: 0 var(--padding-1);
        transition: margin .2s ease-in;
    }
    
    .days input:hover, .magnitude-min input:hover, .depth-min input:hover, .depth-max input:hover {
        margin: 0 var(--padding-2);
    }

    .submit {
        padding: calc(var(--padding-2) / 2);
    }

    .submit input {
        padding: calc(var(--padding-2) / 2);
        border: none;
        background: #FFFFFF;
        cursor: pointer;
        transition: padding .2s ease-in, background .2s ease-in, color .2s ease-in, border-radius .2s ease-in;
    }

    .submit input:hover{
        background: var(--color-darkblue);
        color: var(--color-lightgrey);
        padding: calc(var(--padding-2) / 2) var(--padding-2);
        border-radius: var(--border-radius-2);
    }
</style>