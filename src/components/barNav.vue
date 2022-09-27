<template>
    <div class="EventBar">
        <h1 className='event-bar-title'>Todo</h1>
        <AddEventButton @setValueName="setValueName" ></AddEventButton>
        <div class="firstTag">
            <button class="itemClass tipButton" :class="currentItem ? '' : 'selected-event'" @click = "clickNull">
                Add a new Event
            </button>
        </div>
        <div class="eventName" v-for="(item,index) in eventList" :key="index">
            <button class="itemClass" :class ="item === currentItem ?'selected-event':''" @click="selectItem(item,index)" ref="itemref">
                {{item.eventname}}
            </button> 
        </div>
    </div>
</template>

<script>
    import AddEventButton from "./AddEventButton.vue";
    import { eventBus } from '../main'
    export default {
        data(){
            return {
                eventList:[
                    // {
                    //     eventname: '数学',
                    //     TodoList: [{name: '选择题', detail: '60分'}],
                    //     inProgressList: [{name: '填空题', detail: '25分'}],
                    //     completedList: [{name: '简答题', detail: '22分'},{name: '应用题', detail: '50分'}],
                    // },
                    // {
                    //     eventname: '语文',
                    //     TodoList: [{name: '听力', detail: '20分'}],
                    //     inProgressList: [{name: '填空题', detail: '一元二次'}],
                    //     completedList: [{name: '阅读理解', detail: '20分'},{name: '作文', detail: '60分'}],
                    // },
                ],
                currentItem: null,
                ListIndex:-1
            }
        },
        methods: {
            addEvent() {
            },
            setValueName(data) {
                this.eventList.push({eventname: data,TodoList:[],inProgressList:[],completedList:[]})
                localStorage.setItem("eventList",JSON.stringify(this.eventList))
            },
            selectItem(data,index) {
                this.currentItem = this.eventList[index]
                this.ListIndex = index
                // this.sendDataToKanban()
                // console.log('brotherSaid',this.eventList[index]);
                eventBus.$emit('brotherSaid',this.eventList[index])
            },
            clickNull() {
                this.ListIndex = -1
                this.currentItem = null
                eventBus.$emit('brotherSaid',{})
            },
        },
        mounted() {
            this.eventList = JSON.parse(localStorage.getItem('eventList'))
            eventBus.$emit('brotherSaid',{
                eventname: '',
                TodoList: [],
                inProgressList: [],
                completedList: []})
        },
        components: {
            AddEventButton
        },
        created() {
            eventBus.$on('changedData', data => {
                let tag = data.eventname
                for(let i = 0; i<this.eventList.length; i++) {
                    if(this.eventList[i].eventname === tag) {
                        this.eventList[i] = data
                        localStorage.setItem("eventList",JSON.stringify(this.eventList))
                        break;
                    }
                }
            })
            eventBus.$on('delItem',() => {
                if(this.ListIndex !== -1) {
                    this.eventList.splice(this.ListIndex,1)
                    eventBus.$emit('brotherSaid',{})
                }
                localStorage.setItem("eventList",JSON.stringify(this.eventList))
            })
        },
        beforeDestroy() {
            eventBus.$off('changedData')
        }
    }
</script>

<style scoped>
    
    .EventBar {
        width:300px;
        height: 100%;
        text-align: center;
        border-right: 2px #ededed solid;
    }
    .event-bar-title{
        font-style:italic;
        font-size: 40px;
    }

    .itemClass {
        width: 80%;
        height: 45px;
        font-size: 1.3em;
        line-height: 45px;
        background-color: #eaf1f0;
        border: none;
        margin: 0 auto;
        text-align: center;
        border-radius: 10px;
        cursor: pointer;
        transition: 0.2s;
        outline: none;
        margin-bottom: 20px;
    }
    .itemClass:hover {
        background-color: #dfe2e2;
    }
    .selected-event {
        color: #fff;
        background-color: #e86a3d;
    }
    .tipButton {
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif
    }
</style>