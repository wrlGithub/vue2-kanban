<template>
    <div class="add-button" @click="addEvent">
        +
    </div>
</template>

<script >
    export default {
        data() {
            return {
                eventName: ''
            }
        },  
        methods: {
            addEvent() {
                this.eventName = prompt('请输入事件名称')
                if(this.eventName){
                    this.$emit('setValueName',this.eventName)
                }
            }
        }
    }
</script>

<style scoped>
    .add-button {
        width: 80%;
        height: 45px;
        font-size: 1.3em;
        line-height: 45px;
        background-color: #eaf1f0;
        margin: 0 auto;
        text-align: center;
        border-radius: 10px;
        cursor: pointer;
        transition: 0.5s;
        outline: none;
        margin-bottom: 20px;
    }
    .add-button:hover {
        background-color: #dfe2e2;
    }
</style>
