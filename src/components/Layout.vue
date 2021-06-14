<template>
<div class="layout">
    <!-- header -->
    <header class="header">
        <div class="c-inner">
            <div class="header__wrapper">
                <!-- logo -->
                <h1 class="logo">
                    <router-link to="/" class="logo__link">
                        로고
                    </router-link>
                </h1>
            </div>
        </div>
    </header>
    <!-- content -->
    <main id="contents" class="contents">
        <slot />
    </main>
    <!-- scroll top -->
    <button class="to-top-btn">
        <svg width="11.834" height="18.5" viewBox="0 0 11.834 18.5">
            <path d="M17.86,6.006l-5.188,5.188a.479.479,0,1,1-.678-.678l4.37-4.37H.479a.479.479,0,0,1,0-.958H16.365L11.995.818A.479.479,0,1,1,12.672.14L17.86,5.328A.479.479,0,0,1,17.86,6.006Z" transform="translate(0.25 18.25) rotate(-90)" fill="#222" stroke="#222" stroke-width="0.5"/>
        </svg>
    </button>
    <!-- footer -->
    <footer class="footer" v-if="isFooter">
        <div class="footer__line">
            <div class="c-inner">
                <div class="footer__wrapper">
                </div>
            </div>
        </div>
    </footer>
</div>
</template>

<script>
export default {
    props: {
        isFooter: {
            type: Boolean
        }
    },
    mounted() {
        
    }
}
</script>

<style lang="scss">
@import '../assets/css/common';

.header {
    position: fixed;
    top: 0;
    left: 0;
    background: linear-gradient(180deg, rgba(255,255,255,1) 0%, rgba(255,255,255,0.84) 100%);
    width: 100%;
    z-index: 100;
    transition: .5s;
    background: pink;

    &__wrapper {
        display: flex;
        height: $headerH;
        align-items: center;
        width: 100%;
    }
}

.logo {
    &__link {
        width: 80px;
        overflow: hidden;
        display: block;
        text-indent: 100%;
        text-align: left;
        white-space: nowrap;
        background-size: contain;
        background: url('http://placehold.it/80x50') no-repeat;
    }
}

.to-top-btn {
    position: fixed;
    right: 15px;
    width: 45px;
    height: 45px;
    background: #fff;
    border-radius: 50%;
    bottom: 0;
    transition: 0.5s;
    opacity: 0;
    box-shadow: 0 10px 20px rgba(0,0,0,0.1);
    z-index: 100;

    &.js-on {
        bottom: 15px;
        opacity: 1;
    }
}

.nav {
    flex: 1 1 100%; 
    display: flex;
    justify-content: flex-end;

    &__link {

    }
}

.contents {
    width: 100%;
    position: relative;
    padding-top: $headerH;
}

.footer {
   background: skyblue;
}

</style>