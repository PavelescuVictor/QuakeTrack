<template>
    <div id="app">
        <div id="app__content">
            <div class="navbar__wrapper">
                <Navbar @updatePage="changeDisplayedPage"/>
            </div>
            <div class="content__wrapper">
                <div class="main" v-bind:class="{notDisplayedPage : !isMainActive}">
                    <h1>This is the main page!</h1>
                </div>
                <div class="map" v-bind:class="{notDisplayedPage : !isMapActive}">
                    <Map/>
                </div>
                <div class="globe" v-bind:class="{notDisplayedPage : !isGlobeActive}">
                    <h1>This is where the globe will go.</h1>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Map from "./components/Map.vue";
    import Navbar from "./components/Navbar.vue";
    export default {
        name: "App",
        components: {
            Map,
            Navbar,
        },
        data() {
            return {
                showedPage: 'main',
                isMainActive: true,
                isMapActive: false,
                isGlobeActive: false,
            };
        },
        methods: {
            changeDisplayedPage(e) {
                this.showedPage = e;
            },
        },
        watch: {
            showedPage: function(val) {
                if(val === "main") {
                    this.isMainActive = true;
                    this.isMapActive = false;
                    this.isGlobeActive = false;
                }
                else if(val === "map") {
                    this.isMainActive = false;
                    this.isMapActive = true;
                    this.isGlobeActive = false;
                }
                else if(val === "globe") {
                    this.isMainActive = false;
                    this.isMapActive = false;
                    this.isGlobeActive = true;
                }
            }
        }
    };
</script>

<style>
    /* IMPORTING FONTS */

    @import url('https://fonts.googleapis.com/css2?family=Fjalla+One&family=Pacifico&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Fredoka+One&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@700&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');

    :root{
        /* COLOR VARIABLES */

        --color-green: #00B074;
        --color-lightgreen: #D1F1E6;
        --color-lightgrey: #EDF0EE;
        --color-white: #FFFFFF;
        --color-lightblue: #BEDEDE;
        --color-darkblue: #2B3940;
        --color-grey: #7E8989;

    
        --color-blue: #4C84FF;
        --color-lightgrey-2: #E7E9EE;
        --color-lightgrey-3: #F5F6FA;
        --color-darkblue-2: #454D61;
        --color-green-2: #40CB9E;
        --color-yellow: #F9C108;
        --color-lightyellow: #FFF4CC;

        /* TEXT */

        --textBaseSize: 16px;

        /* FONT VARIABLES */

        --website-font: 'Fjalla One', sans-serif;
        --logo-font: 'Fredoka One', cursive;;
        --nav-font: 'Roboto Condensed', sans-serif;
        /*--nav-font: 'Montserrat', sans-serif;*/
        --font-family-base: Arial, Helvetica, sans-serif;

        
        /* MARGINS */

        --margin-1: 60px;    

        /* PADDINGS */

        --padding-1: 60px;    
        --padding-2: 30px;     
        
        /* BORDER */
        
        --border-radius-1: 15px;
        --border-radius-2: 50px;

    }

    * {
        margin: 0px;
        padding: 0px;
        box-sizing: border-box;
    }

    body, html {
        scroll-behavior: smooth;
        width: 100%;
        height: 100%;
        min-height: 100vh;
        background: var(--color-lightgrey-2);
        font-family: var(--nav-font);
        color: var(--color-darkblue-2);
        font-size: var(--text-base-size);
    }

    a {
        text-decoration: none;
    }

    h1 {
        height: 10vh;
        line-height: 10vh;
    }

    #app {
        height: 100%;
        width: 100%;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
    }

    #app__content {
        min-height: 100vh;
        background: var(--color-lightgrey-2);
        padding: var(--padding-1);
        display: grid;
        grid-template-rows: auto 1fr;
    }

    .navbar__wrapper {
        border-top-left-radius: var(--border-radius-1);
        border-top-right-radius: var(--border-radius-1);
        overflow: hidden;
    }

    .content__wrapper {
        border-bottom-left-radius: var(--border-radius-1);
        border-bottom-right-radius: var(--border-radius-1);
        overflow: hidden;
    }

    .map{
        height: 100%;
        background: var(--color-lightgrey-3);
    }

    /* Main */

    .main{
        height: 100%;
        background: var(--color-lightgrey-3);
    }

    /* Globe */

    .globe{
        height: 100%;
        background: var(--color-lightgrey-3);
    }

    /* UTILITY CLASSES */

    .notDisplayedPage{
        display: none;
    }

</style>
