<template>
    <div class="nav-wrapper">
        <nav class="nav">
            <div class="nav__ul-wrapper">
                <ul class="nav__ul-content">
                    <li>
                        <a href="#">
                            <i class="fas fa-home"></i>
                        </a>
                    </li>
                    <li>
                        <a href="#">
                            <i class="fas fa-map"></i>
                        </a>
                    </li>
                    <li>
                        <a href="#">
                            <i class="fas fa-globe-americas"></i>
                        </a>
                    </li>
                </ul>
            </div>
        </nav>
    </div>
</template>
<script>
    export default {
        name: "Navbar",
    }
</script>
<style scoped>

    a {
        color: var(--color-darkblue-2);
    }

    .nav-wrapper{
        background: #FFFFFF;
    }
    .nav{
        height: 100%;
        margin: auto;
        display: flex;
        justify-content: flex-end;
        align-items: center;
        border-bottom: 6px solid var(--color-lightgrey-2);
    }

    .nav__ul-wrapper {
        height: 100%;
        margin: auto;
    }

    .nav__ul-content {
        height: 100%;
        display:flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        font-size: calc(1.5 * var(--textBaseSize));
        font-family: var(--nav-font);
        color: var(--color-darkblue-2);
        list-style-type: none;
        background: var(--color-lightgrey-2);
    }

    .nav__ul-content li {
        padding: var(--padding-2);
        background: #FFFFFF;
        transition: padding-top .2s ease-in, padding-bottom .2s ease-in, box-shadow .2s ease-in, background 0s ease-in, border-radius .2s ease-in, margin .2s ease-in;
    }

    .nav__ul-content li:hover {
        background: var(--color-lightgrey-3);
        padding-top: calc(.75 * var(--padding-2));
        padding-bottom: calc(1.25 * var(--padding-2));
        box-shadow: 0px 6px 0px 0px var(--color-blue);
        border-radius: 10px;
        margin: 0px 6px;
    }
</style>