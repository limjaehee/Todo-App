<template>
<Layout :isHeader="false">
    <div class="m-message">
        <div class="c-inner">
            <p class="m-message__title">Hello!</p>
            <p class="m-message__name">Sweeney :)</p>
        </div>
    </div>
    <div class="m-banner">
        <div class="c-inner">
            <div class="m-banner__wrapper">
                <strong class="m-banner__ongoing">16</strong>
                <div>
                    <p class="m-banner__message">Tasks for today</p>
                    <p class="m-banner__complete"><span  class="m-banner__complete__num">08</span> complete</p>
                </div>
            </div>
        </div>
    </div>
    
    <!-- ongoing task -->
    <div class="m-task">
        <div class="c-inner">
            <h3 class="m-task__title ongoing">Ongoing Task</h3>
        </div>
        <swipe-list
            ref="list"
            class="card"
            :disabled="!enabled"
            :items="mockSwipeList"
            item-key="id"
            @swipeout:click="itemClick">
            <template v-slot="{ item }">
                <div class="m-task__item">
                    <label class="check">
                        <input type="checkbox">
                        <i class="check__mark icon-check"></i>
                    </label>
                    <div class="m-task__item__txtbox">
                        <p class="m-task__item__task">{{item.task}}</p>
                        <span class="m-task__item__description">{{item.description}}</span>
                    </div>
                    <p class="m-task__item__timebox">
                        <i class="icon-clock"></i>
                        <span>{{item.time}}</span>
                    </p>
                </div>
            </template>
            <template v-slot:right="{ item }">
                <div class="swipeout-action m-task__option">
                    <button class="icon-menu"></button>
                    <button class="icon-delete" @click="remove(item)"></button>
                </div>
            </template>
        </swipe-list>
    </div>

    <hr class="c-hr"/>

    <!-- Completed task -->
    <div class="m-task">
        <div class="c-inner">
            <h3 class="m-task__title completed">Completed Task</h3>
        </div>
        <swipe-list
            ref="list"
            class="card"
            :disabled="!enabled"
            :items="mockSwipeList"
            item-key="id"
            @swipeout:click="itemClick">
            <template v-slot="{ item }">
                <div class="m-task__item">
                    <label class="check">
                        <input type="checkbox" checked>
                        <i class="check__mark icon-check"></i>
                    </label>
                    <div class="m-task__item__txtbox">
                        <p class="m-task__item__task">{{item.task}}</p>
                        <span class="m-task__item__description">{{item.description}}</span>
                    </div>
                    <p class="m-task__item__timebox">
                        <i class="icon-clock"></i>
                        <span>{{item.time}}</span>
                    </p>
                </div>
            </template>
            <template v-slot:right="{ item }">
                <div class="swipeout-action m-task__option">
                    <button class="icon-menu"></button>
                    <button class="icon-delete" @click="remove(item)"></button>
                </div>
            </template>
        </swipe-list>
    </div>
    <div class="end-ui"></div>
    <template #popup>
        <button class="icon-plus m-add-btn"></button>
    </template>
</Layout>
</template>

<script>
//https://github.com/weblineindia/Vue-Swipe-Action
import { SwipeList, SwipeOut } from 'vue-weblineindia-swipe';

export default {
  components: {
    SwipeOut,
    SwipeList
  },
  data() {
    return {
      enabled: true,
      mockSwipeList: [
          {
            task: "Design Homework",
            description: "Hans Teacher",
            time: '12:00',
          },
          {
            task: "Web Coding",
            description: "Anna Teacher",
            time: '13:00',
          },
      ]
    };
  },
  methods: {
    revealFirstRight() {
      this.$refs.list.revealRight(0);
    },
    revealFirstLeft() {
      this.$refs.list.revealLeft(0);
    },
    closeFirst() {
      this.$refs.list.closeActions(0);
    },
    closeAll() {
      this.$refs.list.closeActions();
    },
    remove(item) {
      this.mockSwipeList = this.mockSwipeList.filter(i => i !== item);
      // console.log(e, 'remove');
    },
    itemClick(e) {
      console.log(e, "item click");
	},
    fbClick(e) {
      console.log(e, "First Button Click");
    },
    sbClick(e) {
      console.log(e, "Second Button Click");
	},
  },
};

</script>

<style lang="scss">

@import '../../assets/css/common';

.swipeout {
  position: relative;
  overflow: hidden;
  display: flex;
}

.swipeout .swipeout-left,
.swipeout .swipeout-right {
  position: absolute;
  height: 100%;
  display: flex;
  z-index: 1;
}

.swipeout .swipeout-left {
  left: 0;
  transform: translateX(-100%);
}

.swipeout .swipeout-right {
  right: 0;
  transform: translateX(100%);
}

.swipeout .swipeout-content,
.swipeout .swipeout-action {
  transition: transform 0.2s;
}

.swipeout.swipeout--no-transition .swipeout-content,
.swipeout.swipeout--no-transition .swipeout-action {
  transition: none !important;
}

.swipeout .swipeout-content {
  width: 100%;
}

.swipeout-non-selectable {
  user-select: none !important;
}

.swipeout-no-pointer-events {
  pointer-events: none !important;
}

/* SwipeList */

.swipeout-list {
  display: flex;
  flex-direction: column;
}

.swipeout-list-item {
  flex: 1;
}

//메시지
.m-message {
    padding: 40px 0 25px 0;

    &__title {
        color: #999;
        font-size: $fz20;
        font-weight: 600;
    }

    &__name {
        color: $f-color;
        font-size: $fz20;
        font-weight: 600;
    }
}

//베너
.m-banner {
    position: relative;
    z-index: 1;

    &::after {
        content: "";
        display: block;
        position: absolute;
        left: 0;
        bottom: -6vh;
        height: 6vh;
        background: linear-gradient(180deg, rgba(255,255,255,1) 0%, rgba(255,255,255,0) 100%);
        width: 100%;
        z-index: -1;
    }

    &__wrapper {
        background: #f6f6fb;
        border-radius: 10px;
        box-shadow: 0 3px 10px rgba(0,0,0,0.05);
        padding: .7rem 1.2rem;
        box-sizing: border-box;
        display: flex;
        align-items: center;
    }

    &__ongoing {
        color: $c-color;
        font-size: $fz50;
        font-weight: 500;
        margin-right: 7%;
    }

    &__message {
        color: $f-color;
        font-weight: 600;
        margin-bottom: 2px;
    }

    &__complete {
        color: #999;
        font-size: $fz14;

        &__num {
            color: $c-color;
            font-weight: 400;
            margin-right: 5px;
        }
    }
}

//할일
.m-task {
    padding: 25px 0 15px 0;

    //타이틀
    &__title {
        font-weight: 600;
        color: $f-color;
        margin-bottom: 1rem;

        &::before {
            content: "";
            display: inline-block;
            width: 15px;
            height: 15px;
            border: 1px solid #000;
            border-radius: 2px;
            margin-right: 10px;
        }

        &.ongoing {
            &::before {
                background: #f3e3f9;
                border-color: #f2dafb;
            }
        }
        
        &.completed {
            &::before {
                background: #e3e3f9;
                border-color: #dadafe;
            }
        }
    }

    //내용
    &__item {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        padding: .4rem 4%;

        &__txtbox {
            flex: 1;
            margin-left: 10px;
        }

        &__task {
            font-size: $fz16;
            color: $f-color;
            font-weight: 500;
            line-height: 1.3;
        }

        &__description {
            color: #c6ccd8;
            font-size: $fz12;
        }

        &__timebox {
            color: #a2a2a2;
            display: flex;
            align-items: center;
            font-size: $fz12;

            .icon-clock {
                margin-right: 5px;
                font-size: 14px;
            }
        }
    }

    //option
    &__option {
        margin-top: 5px;

        .icon-menu {
            color: #dfdfdf;
            font-size: 20px;
            margin-left: 10px;
            width: 24px;
            height: 24px;
        }

        .icon-delete {
            color: #ff6d6d;
            font-size: 20px;
            margin-left: 10px;
            margin-right: 20px;
            width: 24px;
            height: 24px;
        }
    }
}

//end
.end-ui {
    width: 100%;
    height: 6vh;

    &::after {
        content: "";
        width: 100%;
        position: fixed;
        height: 8vh;
        bottom: 0;
        left: 0;
        background: linear-gradient(180deg, rgba(255,255,255,0.5) 0%, rgba(255,255,255,1) 100%);
    }
}

.m-add-btn {
    position: fixed;
    bottom: 2rem;
    right: 4%;
    width: 40px;
    height: 40px;
    background: linear-gradient(225deg, rgba(124,120,229,1) 0%, rgba(176,130,236,1) 100%);
    border-radius: 10px;
    color: #fff;
    font-size: 18px;
    box-shadow: 0 3px 6px rgba(0,0,0,0.16);
}
</style>