<template>
  <div id="app">
    <!-- page-loader-->
    <div class="page-loader" v-if="loader">
      <div class="page-loader__item">Loading...</div>
    </div>
    <!-- end page-loader -->

    <!-- skip-nav -->
    <div class="skip-nav">
      <a href="#contents" class="skip-nav__link">본문 바로가기</a>
    </div>
    <!-- end skip-nav -->

    <router-view></router-view>
  </div>
</template>

<script>
export default {
    name: 'app',
    data () {
		return {
			loader: false
		}
    }
}
</script>

<style lang="scss">
@import './assets/css/reset';
@import './assets/css/common';

.page-loader {
    display: block;
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    background: #fefefe;
    z-index: 10000;

	&__item {
		width: 50px;
		height: 50px;
		position: absolute;
		top: 50%;
		left: 50%;
		margin: -25px 0 0 -25px;
		font-size: 10px;
		text-indent: -12345px;
		border-top: 1px solid rgba(0,0,0, 0.08);
		border-right: 1px solid rgba(0,0,0, 0.08);
		border-bottom: 1px solid rgba(0,0,0, 0.08);
		border-left: 1px solid rgba(0,0,0, 0.5);
		-webkit-border-radius: 50%;
		-moz-border-radius: 50%;
		border-radius: 50%;
		-webkit-animation: spinner 700ms infinite linear;
		-moz-animation: spinner 700ms infinite linear;
		-ms-animation: spinner 700ms infinite linear;
		-o-animation: spinner 700ms infinite linear;
		animation: spinner 700ms infinite linear;
		z-index: 10001;
	}
}

@keyframes spinner {
	0% {
		-webkit-transform: rotate(0deg);
		-moz-transform: rotate(0deg);
		-ms-transform: rotate(0deg);
		-o-transform: rotate(0deg);
		transform: rotate(0deg);
	}
	100% {
		-webkit-transform: rotate(360deg);
		-moz-transform: rotate(360deg);
		-ms-transform: rotate(360deg);
		-o-transform: rotate(360deg);
		transform: rotate(360deg);
	}
}

.skip-nav {
	&__link {
		overflow: hidden;
		position: absolute;
		width: 0;
		height: 0;
	}
}

</style>
