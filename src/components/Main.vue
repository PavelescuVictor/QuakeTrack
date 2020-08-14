<template>
    <div class="main">
        <section class="banner">
            <div class="banner-inner">
                <div class="banner-inner-content">
                    <h1>Quake Track</h1>
                    <p>A responsive platform to check out earthquakes on earth</p>
                    <p>Delivering the latest news and recent weather forecast</p>
                    <div class="more-btn">
                        <a href="https://www.google.com">Learn More</a>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>
<script>
export default {
    name: "Main"
}
</script>
<style scoped>
    .banner {
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        background-image: url("../assets/earth-night-map.jpg");
        background-position: center;
        background-size: cover;
        background-repeat: no-repeat;
        position: relative;
        overflow: hidden;
    }

    .banner-inner {
        z-index: 2;
        height: 100%;
        width: 100%;
        display: flex;
        flex-direction: column;
    }

    .banner-inner-content{
        flex: 0;
        text-align: center;
        margin: auto;
        padding: var(--padding-1);
    }

    .banner-inner-content h1 {
        height: calc(var(--textBaseSize) * 7);
        margin: var(--padding-2);
        line-height: calc(var(--textBaseSize) * 7);
        color: #fff;
        font-size: calc(var(--textBaseSize) * 7);
        font-weight: 400;
        font-family: var(--logo-font);
        animation: banner__text__logo 1s 1;
    }

    .banner-inner-content p {
        padding: calc(var(--padding-2) / 4);
        color: rgba(255, 255, 255, 1);
        font-size: calc(var(--textBaseSize) * 1.4);
        font-weight: 300;
        animation: banner__text__p 1s 1;
    }

    .banner .banner-inner .banner-inner-content .more-btn {
        width: 6em;
        margin: auto;
        padding: calc(var(--padding-2) / 3);
        margin-top: 1em;
        background: var(--color-blue);
        font-size: calc(var(--textBaseSize) * 1.3);
        border-radius: var(--border-radius-2);
        transition: width 0.2s ease-in-out;
        animation: banner__text__button 1s 1;
    }

    .banner .banner-inner .banner-inner-content .more-btn:hover {
        width: 7.5em;
    }

    .banner .banner-inner .banner-inner-content .more-btn a {
        color: #111111;
        transition: color 0.2s ease-in;
    }

    .banner .banner-inner .banner-inner-content .more-btn:hover > a {
        color: rgba(255, 255, 255, 1);
    }

    .banner:before {
        transition: opacity 3s ease;
        transition-delay: 1s;
        content: '';
        background-color: #42424f;
        display: block;
        height: 100%;
        left: 0;
        opacity: 0.45;
        position: absolute;
        top: 0;
        width: 100%;
        z-index: 1;
    }

    @keyframes banner__text__p{
        0%{
            opacity: 0%;
            margin-top: -3em;
        }

        30%{
            letter-spacing: 0.05em;

        }

        40%{
            opacity: 0%;
        }

        50%{
            letter-spacing: -0.1em;
        }
    }


    @keyframes banner__text__logo{
        0%{
            opacity: 0%;
        }

        30%{
            letter-spacing: 0.05em;
        }

        50%{
            letter-spacing: -0.1em;
        }
    }


    @keyframes banner__text__button{
        0%{
            opacity: 0%;
        }

        30%{
            width: 10em;
            letter-spacing: -0.1em;
        }

        100%{
            width: 6em;
        }

    }
</style>