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
                <strong class="m-banner__ongoing">{{OngoingLength()}}</strong>
                <div>
                    <p class="m-banner__message">Tasks for today</p>
                    <p class="m-banner__complete"><span  class="m-banner__complete__num">{{CompletedLength()}}</span> complete</p>
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
            :items="OngoingList"
            item-key="id"
            @swipeout:click="itemClick">
            <template v-slot="{ item, index }">
                <div class="m-task__item">
                    <label class="check">
                        <input type="checkbox" 
                            v-model="item.ischeck" 
                            :checked="OngoingCEvt(index,item.ischeck)">
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
            <template v-slot:right="{ item, index }">
                <div class="swipeout-action m-task__option">
                    <button class="icon-menu" @click="PopupOn(index,item)"></button>
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
            :items="CompletedList"
            item-key="id"
            @swipeout:click="itemClick">
            <template v-slot="{ item, index }">
                <div class="m-task__item">
                    <label class="check">
                        <input type="checkbox"
                            v-model="item.ischeck"
                            :checked="CompletedCEvt(index,item.ischeck)">
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
                    <button class="icon-delete" @click="remove(item)"></button>
                </div>
            </template>
        </swipe-list>
    </div>
    <div class="end-ui"></div>
    <!-- popup -->
    <template #popup>
        <button class="icon-plus m-add-btn" @click="AddPopup=true"></button>
        <transition name="fade">
            <AddPopup
                v-if="AddPopup"
                class="task-popup"
                :Width="'90%'"
                :title="'Enter task'"
                :BtnClass="BtnClass()"
                @BtnEvt="BtnEvt()"
                @close="Close()">
                <template #cont>
                    <div class="task-popup__cont">
                        <div class="c-input" :class="`${isCheck.title ? 'clear' : ''}`">
                            <input type="text" class="c-input__input"
                                v-model="newTask.title"
                                @blur="isCheck.title = true"/>
                            <label for="sampleId" class="c-input__title">Enter your task</label>
                        </div>
                        <div class="c-input" :class="`${isCheck.description ? 'clear' : ''}`">
                            <input type="text" class="c-input__input"
                                v-model="newTask.description"
                                @blur="isCheck.description = true"/>
                            <label for="sampleId" class="c-input__title">Enter your description</label>
                        </div>
                    </div>
                </template>
            </AddPopup>
        </transition>
    </template>
</Layout>
</template>

<script>
//https://github.com/weblineindia/Vue-Swipe-Action
import { SwipeList, SwipeOut } from 'vue-weblineindia-swipe';
import AddPopup from './component/AddPopup';

export default {
    components: {
        SwipeOut,
        SwipeList,
        AddPopup
    },
    data() {
        return {
            enabled: true,
            //팝업
            AddPopup: false,
            newTask: {
                title: '',
                description:'',
            },
            isCheck: {
                title: '',
                description: '',
            },
            //추가된 task
            OngoingList: [
            //    {
            //         task: "Design Homework",
            //         description: "Hans Teacher",
            //         time: '12:00',
            //         ischeck: false,
            //     },
            //     {
            //         task: "Web Coding",
            //         description: "Anna Teacher",
            //         time: '13:00',
            //         ischeck: false,
            //     },
            //     {
            //         task: "Ui Design",
            //         description: "Hans Teacher",
            //         time: '12:00',
            //         ischeck: false,
            //     },
            ],
            //완료된 task
            CompletedList: [
                // {
                //     task: "Design Homework",
                //     description: "Hans Teacher",
                //     time: '12:00',
                //     ischeck: true,
                // },
                // {
                //     task: "Web Coding",
                //     description: "Anna Teacher",
                //     time: '13:00',
                //     ischeck: true,
                // },
                // {
                //     task: "Ui Design",
                //     description: "Hans Teacher",
                //     time: '12:00',
                //     ischeck: true,
                // },
            ],
            //setting
            settingIndex: 0,
            settingOn: false,
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
            this.OngoingList = this.OngoingList.filter(i => i !== item);
            this.CompletedList = this.CompletedList.filter(i => i !== item);
        },
        itemClick(e) {
            //console.log(e, "item click");
        },
        fbClick(e) {
            console.log(e, "First Button Click");
        },
        sbClick(e) {
            console.log(e, "Second Button Click");
        },
        //체크할경우
        OngoingCEvt(evt, ischeck) {
            if(ischeck == true) {
                setTimeout(()=>{
                    //푸쉬를 먼저 하고
                    this.CompletedList.push({
                        task: this.OngoingList[evt].task,
                        description: this.OngoingList[evt].description,
                        time: this.OngoingList[evt].time,
                        ischeck: true
                    })
                    //잘라내는 방식
                    this.OngoingList.splice(evt, 1);
                },500)
            }
        },
        //체크해제할경우
        CompletedCEvt(evt, ischeck) {
            if(ischeck == false) {
                //푸쉬를 먼저 하고
                this.OngoingList.push({
                    task: this.CompletedList[evt].task,
                    description: this.CompletedList[evt].description,
                    time: this.CompletedList[evt].time,
                })
                //잘라내는 방식
                this.CompletedList.splice(evt, 1);
            }
        },
        //task 개수
        OngoingLength() {
            const i = this.OngoingList.length
            if(i<10&&!i==0) {
                return '0'+i
            }else {
                return i
            }
        },
        //완료된 task 개수
        CompletedLength() {
            const i = this.CompletedList.length
            if(i<10&&!i==0) {
                return '0'+i
            }else {
                return i
            }
        },
        //팝업 버튼 활성화
        BtnClass() {

            if(!!this.newTask.title.trim()) {
                if(!!this.newTask.description.trim()) {
                    return 'clear'
                }
            }
            
        },
        //완료 버튼 클릭
        BtnEvt() {
            //time
            const date = new Date();
            const minutes = date.getMinutes();
            const hours = date.getHours();
            const title = this.newTask.title;

            //새로 push일 경우
            if(this.settingOn == false && !title == '') {
                this.OngoingList.push({
                    task: this.newTask.title,
                    description: this.newTask.description,
                    time: `${hours < 10 ? `0${hours}`: hours}:${minutes < 10 ? `0${minutes}`:minutes}`,
                })
            }else if(this.settingOn == true && !title == '') {
                //기존 data setting일 경우
                this.OngoingList[this.settingIndex].task = this.newTask.title;
                this.OngoingList[this.settingIndex].description = this.newTask.description;
                this.OngoingList[this.settingIndex].time = `${hours < 10 ? `0${hours}`: hours}:${minutes < 10 ? `0${minutes}`:minutes}`;
                this.settingOn = false
            }

            this.AddPopup = false
            this.reset()
        },
        //설정 클릭시
        PopupOn(i,item) {
            this.AddPopup = true;

            //팝업에 데이터 표시
            this.newTask.title = item.task;
            this.isCheck.title = item.task;
            this.newTask.description = item.description;
            this.isCheck.description = item.description;

            //setting값
            this.settingOn = true;
            this.settingIndex = i
        },
        //close btn
        Close() {
            this.AddPopup = false;
            this.reset();
        },
        //reset
        reset() {
            this.newTask.title = '';
            this.isCheck.title = '';
            this.newTask.description = '';
            this.isCheck.description = '';
        }
    },
    computed: {

    },
    mounted() {
        const banner = document.querySelector('.m-banner');
        const message = document.querySelector('.m-message');
        const bannerY = banner.getBoundingClientRect().top
        
        window.addEventListener('scroll', function(){
            const scrollY = window.scrollY;

            if(scrollY >= bannerY) {
                banner.classList.add('on')
                message.classList.add('on')
            }else {
                banner.classList.remove('on')
                message.classList.remove('on')
            }
        });
    }
};

</script>

<style lang="scss">

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

    &.on {
        padding-bottom: 120px;
    }

    &__title {
        color: #999;
        font-size: $fz20;
        font-weight: 600;
        line-height: 1.1;
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

    &.on {
        position: fixed;
        top: 0;
        left: 0;
        padding-top: 1rem;
        width: 100%;
        z-index: 10;
        background: #fff;
    }

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
        background: linear-gradient(180deg, rgba(255,255,255,0.2) 0%, rgba(255,255,255,1) 100%);
    }
}

//추가 버튼
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

//팝업
.task-popup {
    &__cont {
        padding: 1.5rem 1.5rem 0;
        box-sizing: border-box;
    }
}

.c-input {
    box-sizing: border-box;
    width: 100%;
    height: 51px;
    justify-content: center;
    position:relative; 
    margin-bottom: 1.5rem;
    
    &__title {
        color: #cbcbcb;
        font-size: $fz12;
        position:absolute; 
        left:0; 
        bottom:0; 
        padding:0 6px; 
        color:#999; 
        font-weight:normal; 
        background:#fff; 
        transform:scale(1) translate(4px, -18px); 
        transition:all .15s; 
        pointer-events:none;
    }

    &__input {
        font-size: $fz14;
        outline:none;
        height: 51px;
        padding: 1rem;
        width: 100%;
        box-sizing: border-box;
        border: 1px solid #eee;
        border-radius: 2px;

        &::placeholder {
            color: #cbcbcb;
        }

        &:focus {
            border:1px solid $c-color;
            &~.c-input__title {
                transform:scale(.85) translate(-10px, -48px);
            }
        }
    }

    &.clear {
        .c-input__title {
            transform:scale(.85) translate(-10px, -48px);
        }
    }
}

</style>