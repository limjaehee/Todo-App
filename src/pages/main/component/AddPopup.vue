<template>
  <div class="c-popup">
    <div class="black-bg"></div>
    <div class="c-popup__cont" :style="{ 'max-width': Width }">
      <slot name="sideCont"></slot>
      <div class="c-popup__cont__wrapper">
        <div class="c-popup__cont__header">
          <strong class="c-popup__cont__title">{{ title }}</strong>
          <i class="icon-close" @click="$emit('close')"></i>
        </div>
        <slot name="cont"> </slot>
        <div class="c-popup__cont__footer">
          <button class="c-btn" @click="$emit('close')">Close</button>
          <button class="c-btn full" :class="BtnClass" @click="$emit('BtnEvt')">
            Confirm
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    Width: {
      type: String
    },
    title: {
      type: String
    },
    BtnClass: {
      type: String
    }
  }
};
</script>

<style lang="scss">
//팝업 공통
.c-popup {
  position: fixed;
  width: 100%;
  height: 100%;
  display: flex;
  z-index: 100;
  left: 0;
  top: 0;

  &__cont {
    background: #fff;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    border-radius: 15px;
    overflow: hidden;
    border: 1px solid #dcdcdc;
    box-sizing: border-box;
    display: flex;
    justify-content: space-between;
    width: 90%;

    &__wrapper {
      width: 100%;
      height: 100%;
    }

    &__header {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 100%;
      padding: 20px 0;
      background: #fafafe;
      box-sizing: border-box;

      .icon-close {
        @include font-size(18px);
        cursor: pointer;
        position: absolute;
        right: 1.5rem;
        color: #707070;
      }
    }

    &__footer {
      display: flex;
      justify-content: space-between;
      box-sizing: border-box;
      align-items: center;
      width: 100%;
      padding: 17px 1.5rem;
      border-top: 1px solid #eee;
    }

    &__title {
      @include font-size(18px);
      color: $f-color;
    }
  }
}
</style>
