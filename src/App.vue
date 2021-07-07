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

//넓이값 공통
.c-inner { 
	padding: 0 4%;
	margin: 0 auto;
}

.skip-nav {
	&__link {
		overflow: hidden;
		position: absolute;
		width: 0;
		height: 0;
	}
}

.c-hr {
	background: #f9f9f9;
	border: none;
	height: 6px;
}

//checkbox
.check {
    position: relative;
    cursor: pointer;
    font-size: $fz18;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    line-height: 21px;
    display: inline-flex;
    align-items: center;

    input {
        position: absolute;
        opacity: 0;
        cursor: pointer;
        height: 0;
        width: 0;
    }

    &__mark {
        height: 20px;
        width: 20px;
        border-radius: 50%;
        border: 1px solid #dcdcdc;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
		color: #fff;
		font-size: 16px;
    }

    input:checked ~ &__mark {
        background-color: #aface9;
        border-color: #aface9;
    }

    input:checked ~ &__mark::after {
        content: "";
        display: block;
        width: 45px;
        height: 45px;
        position: absolute;
        left: 50%;
        top: 50%;
		transform: translate(-50%, -50%);
        background: #aface9;
        border-radius: 50%;
        animation: check 1 1s forwards;
    }

    @keyframes check {
        from {
            transform: translate(-50%, -50%) scale(0.2);
			transform-origin: center;
			
        }
        to {
            transform: translate(-50%, -50%) scale(1);
			transform-origin: center;
            opacity: 0;
            visibility: hidden;
        }
    }
}

//배경 공통
.black-bg {
	position: absolute;
	width: 100%;
	height: 100%;
	background: rgba(0,0,0,0.5);
}

//버튼 스타일 정의 공통
.c-btn {
	border-radius: 4px;
	border: 1px solid #eee;
	padding: .5rem 1rem;
	min-width: 100px;
	color: #847ae6;
	font-size: $fz16;
	display: flex;
	display: inline-block;

	&:hover,&:active {
		background: #F8F8F8;
	}

	i {
		margin-right: 15px;
		//font-size: $fz16;
	}

	//그레이배경
	&.full {
		background: #eee;
		color: #fff;
		border-color: #eee;

		&:hover,&:active {
			background: #e6e6e6;
		}
	}

	//컬러 배경
	&.clear {
		background: $c-color;
		border-color: $c-color;
		color: #fff;
        box-shadow: 0 3px 6px rgba(132,122,230,.39);

        &:hover,&:active {
			background: $c-color;
		}

		i {
			color: #fff;
		}
	}
}

//트렌지션
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s
}

.fade-enter, .fade-leave-active {
  opacity: 0
}
</style>
