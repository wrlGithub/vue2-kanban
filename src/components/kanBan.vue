<template>
    <div class="kanban">
        <div class="delEventClass">
            <p class="titleclass">All Tasks</p>
            <button class="delEventButton" @click="delEvent">Delete event</button>
        </div>
        <div class="itxst">
            <!-- To do -->
            <div class="col">
                <div class="title">To do</div>
                <button class="addButton" @click="addInTodoItem">+</button>
                <draggable v-model="curEvent.TodoList" group="site" animation="300" dragClass="dragClass" ghostClass="ghostClass" chosenClass="chosenClass" @start="onStart" @end="onEnd" @add="onAdd">
                    <transition-group :style="style">
                        <div class="item" v-for="(item,index) in curEvent.TodoList" :key="index" ref="todoItem">
                            <h2>{{item.name}}  </h2> 
                            <p>{{item.detail}}</p>
                            <div class="delItemBtn" title = "删除该任务" @click="onDeleteTodo(index)"></div>
                        </div>
                    </transition-group>
                </draggable>
            </div>
            <!-- In progress -->
            <div class="col">
                <div class="title">In progress</div>
                <button class="addButton" @click="addInProgressItem">+</button>
                <draggable v-model="curEvent.inProgressList" group="site" animation="300" dragClass="dragClass" ghostClass="ghostClass" chosenClass="chosenClass" @start="onStart" @end="onEnd" @add="onAdd">
                    <transition-group :style="style">
                        <div class="item" v-for="(item,index) in curEvent.inProgressList" :key="index">
                            <h2>{{item.name}}  </h2> 
                            <p>{{item.detail}}</p>
                            <div class="delItemBtn" title = "删除该任务" @click="onDeletePro(index)"></div>
                        </div>
                    </transition-group>
                </draggable>
            </div>
            <!-- Completed -->
            <div class="col">
                <div class="title">Completed</div>
                <button class="addButton" @click="addCompletedItem">+</button>
                <draggable v-model="curEvent.completedList" group="site" animation="300" dragClass="dragClass" ghostClass="ghostClass" chosenClass="chosenClass" @start="onStart" @end="onEnd"  @add="onAdd">
                    <transition-group :style="style">
                        <div class="item" v-for="(item,index) in curEvent.completedList" :key="index">
                            <h2>{{item.name}}  </h2> 
                            <p>{{item.detail}}</p>
                            <div class="delItemBtn" title = "删除该任务" @click="onDeleteCom(index)"></div>
                        </div>
                    </transition-group>
                </draggable>
            </div>
        </div>
    </div>

</template>

<script>
    import draggable from 'vuedraggable'
    import { eventBus } from '../main'
    export default {
        data() {
            return {
                curEvent: {
                        // eventname: '数学',
                        // TodoList: [{name: '填空题', detail: '一元二次'},{name: '判断题', detail: '一元二次'},{name: '函数', detail: '一元二次'}],
                        // inProgressList: [{name: '证明题', detail: '一元二次'}],
                        // completedList: [{name: '多选题', detail: '一元二次'}],
                    },
                    drag: true,
                    style:'min-height:120px;display: block;'
            }
        },
        methods: {
            onStart() {
                this.drag = true;
            },
            onEnd() {
                this.drag = false;
            },
            onadd() {
                console.log(this.curEvent);
            },
            onDeleteTodo(index) {
                this.curEvent.TodoList.splice(index,1)
                eventBus.$emit('changedData',this.curEvent)
            },
            onDeletePro(index) {
                this.curEvent.inProgressList.splice(index,1)
                eventBus.$emit('changedData',this.curEvent)
            },
            onDeleteCom(index) {
                this.curEvent.completedList.splice(index,1)
                eventBus.$emit('changedData',this.curEvent)
            },
            addInTodoItem() {
               let name = prompt('请输入项目名称')
               let detail
               if(name) {
                    detail = prompt('请输入项目详细')
                    this.curEvent.TodoList.push({name,detail});
                    this.onAdd()
               }
            },
            addInProgressItem() {
                let name = prompt('请输入项目名称')
                let detail
                if(name) {
                        detail = prompt('请输入项目详细')
                        this.curEvent.inProgressList.push({name,detail});
                }
            },
            addCompletedItem() {
                let name = prompt('请输入项目名称')
                let detail
                if(name) {
                        detail = prompt('请输入项目详细')
                        this.curEvent.completedList.push({name,detail});
                }
            },
            onAdd() {
                // 拖拽导致task的位置改变的时候发送数据给nav，说明原数组改变了
                eventBus.$emit('changedData',this.curEvent)
            },
            delEvent() {
                eventBus.$emit('delItem')
            }
        },
        created() {
            eventBus.$on('brotherSaid',(msg) =>{
                this.curEvent = msg
            })
        },
        computed:{
            curEventCom() {
                return this.curEvent
            }
        },
        components: { draggable }
    }
</script>

<style scoped>
    *{
        margin: 0;
        padding: 0;
    }
    /*定义要拖拽元素的样式*/
    .ghostClass {
        background-color: blue !important;
    }

    .chosenClass {
        background-color: rgb(104, 102, 102) !important;
        opacity: 1 !important;
    }

    .dragClass {
        background-color: rgb(226, 168, 43) !important;
        opacity: 1 !important;
        box-shadow: none !important;
        outline: none !important;
        background-image: none !important;
    }

    .itxst {
        margin: 10px;
        display: flex;
        
    }

    .title {
        padding: 6px 12px;
    }

    .col {
        width: 20%;
        flex: 1;
        padding: 10px;
        border: solid 1px #eee;
        border-radius: 5px;
        float: left;
    }

    .col + .col {
        margin-left: 10px;
    }

    .item {
        padding: 6px 12px;
        margin: 0px 10px 0px 10px;
        border: solid 1px #eee;
        background-color: #f1f1f1;
        position: relative;
    }

    .item:hover {
        background-color: #fdfdfd;
        cursor: move;
    }

    .item + .item {
        border-top: none;
        margin-top: 6px;
    }

    .list-enter-active,
    .list-leave-active {
    transition: all 0.5s ease;
    }

    .list-enter-from,
    .list-leave-to {
    opacity: 0;
    transform: translateX(30px);
    }

    .addButton {
        width: 94%;
        height: 40px;
        border: none;
        border-radius: 6px;
        margin-bottom: 10px;
        font-size: 30px;
    }
    .addButton:hover {
        background-color: rgb(222,228,228)
    }
    .delEventClass {
        margin-bottom: 20px;
        width: 50%;
        display: flex;
        align-items: center;
        
    }
    .delEventButton {
        width: 200px;
        height: 40px;
        border: none;
        border-radius: 8px;
        margin-bottom: 10px;
        font-size: 20px;
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif
    }
    .delEventButton:hover {
        background-color: red;
        color: #eee;
    }
    .titleclass {
        display: block;
        margin-right: 30px;
        margin-left: 30px;
        font-size: 35px;
        line-height: 100%;
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif
    }
    .delItemBtn {
        width: 25px;
        height: 7px;
        position: absolute;
        right: 10px;
        bottom: 15px;
        font-size: 20px;
        font-weight: 900;
        text-align: end;
        cursor: pointer;
        background-color: rgb(120, 67, 67);
        border-radius: 2px;
    }
    .delItemBtn:hover {
        color: red;
        width: 40px;
        height: 15px;
        background-color: red;
        font-size: 25px;
        line-height: 10px;
        font-weight: 900;
        transition: 0.5s;
        content: attr(title);
        border-radius: 4px;
    }
    .item > p {
        display: block;
        margin-top: 15px;
    }
</style>
