<template>
    <div class="globe">
        <div class="title">
            <p>Global Representation</p>
        </div>
        <div id="globe__wrapper">
        </div>
    </div>
</template>
<script>
    import Globe from 'globe.gl';
    import * as d3 from 'd3';
    import axios from 'axios';
    import * as THREE from 'three';
    export default {
        name: "Globe",
        data () {
            return {
                globe: null,
                neverShowed: true,
                globeWidth: null,
                globeHeight: null,
                days: 1,
            }
        },
        props: ['isGlobeActive'],
        methods: {
            showHexInfo(e){
                console.log(e);
            }
        },
        component: {
            Globe,
        },
        watch: {
            isGlobeActive: function() {
                if(this.isGlobeActive === true && this.neverShowed === true){

                    var globeContainer = document.getElementById("globe__wrapper");
                    this.globeHeight = globeContainer.clientHeight;
                    this.globeWidth = globeContainer.clientWidth;

                    const weightColor = d3.scaleLinear()
                    .domain([0, 60])
                    .range(['lightblue', 'darkred'])
                    .clamp(true);

                    this.globe = Globe()(globeContainer);

                    this.globe
                    .width(this.globeWidth)
                    .height(this.globeHeight)
                    .globeImageUrl('https://cors-anywhere.herokuapp.com/https://www.nasa.gov/specials/blackmarble/2012/globalmaps/BlackMarble_2012_3km.jpg')
                    .bumpImageUrl('//unpkg.com/three-globe/example/img/earth-topology.png')
                    .backgroundImageUrl('//cdn.jsdelivr.net/npm/three-globe/example/img/night-sky.png')
                    .showAtmosphere(true)
                    .showGraticules(true)
                    .hexBinPointLat(d => d.geometry.coordinates[1])
                    .hexBinPointLng(d => d.geometry.coordinates[0])
                    .hexBinPointWeight(d => d.properties.mag * 10)
                    .hexAltitude(({ sumWeight }) => sumWeight * 0.0025)
                    .hexTopColor(d => weightColor(d.sumWeight))
                    .hexSideColor(d => weightColor(d.sumWeight))
                    .hexBinResolution(2)
                    .hexMargin(0.4)
                    .hexTopCurvatureResolution(10)
                    .hexLabel(d => `
                        <b>${d.points.length}</b> earthquakes in the past month:<ul><li>
                        ${d.points.slice().sort((a, b) => b.properties.mag - a.properties.mag).map(d => d.properties.title).join('</li><li>')}
                        </li></ul>
                    `)
                    .onHexClick(this.showHexInfo)

                    var currentdate = new Date();
                    var dateTime = `${currentdate.getFullYear()}-${(currentdate.getMonth()+1)}-${currentdate.getDate()}T${currentdate.getHours()}:${currentdate.getMinutes()}:${currentdate.getSeconds()}`
                    var queryDate = new Date();
                    queryDate.setDate(currentdate.getDate() - this.days);
                    var queryDateTime = `${queryDate.getFullYear()}-${(queryDate.getMonth()+1)}-${queryDate.getDate()}T${queryDate.getHours()}:${queryDate.getMinutes()}:${queryDate.getSeconds()}`
                    axios.get(`https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime=${queryDateTime}&endtime=${dateTime}`)
                    .then(equakes => {
                       this.globe.hexBinPointsData(equakes.data.features);
                    });


                    // custom globe material
                    const globeMaterial = this.globe.globeMaterial();
                    globeMaterial.bumpScale = 10;
                    new THREE.TextureLoader().load('//unpkg.com/three-globe/example/img/earth-water.png', texture => {
                        globeMaterial.specularMap = texture;
                        globeMaterial.specular = new THREE.Color(0x362541);
                        globeMaterial.shininess = 5;
                    });

                    setTimeout(() => { // wait for scene to be populated (asynchronously)
                        const directionalLight = this.globe.scene().children.find(obj3d => obj3d.type === 'DirectionalLight');
                        directionalLight && directionalLight.position.set(1, 1, 1); // change light position to see the specularMap's effect
                    });

                    // Add auto-rotation
                    //this.globe.controls().autoRotate = true;
                    //this.globe.controls().autoRotateSpeed = 0.6;
                    this.globe.controls().enablePan = true;
                    this.neverShowed = false;
                }
            },
        }
    }
</script>
<style scoped>
    .globe {
        height: 100%;
    }

    .title {
        position: absolute;
        color: white;
        z-index: 2;
        font-size: 2rem;
        bottom:0;
        left:0;
        margin: var(--margin-1) var(--margin-1);
        padding: var(--padding-2) var(--padding-2)
    }
    
    #globe__wrapper {
        height: 100%;
        width: 100%;
    }
</style>