<template>
    <div>
        <div id="screen" :class="state" @click="onClickScreen">{{message}}</div>
        <template v-if="result.length">
            <div>평균 시간 : {{average}}ms</div>
            <button @click="onReset">초기화</button>
        </template>
    </div>
</template>

<script>
    let startTime = 0;
    let endTime = 0;
    let timeOut = null;
    export default {
        data(){
            return {
                result: [],
                state: 'waiting',
                message: '클릭해서 시작하세요.',
            }
        },
        computed:{//일반 데이터를 가공하여 사용할 때
            average(){
                return this.result.reduce((a, c) => a+c, 0) / this.result.length || 0;
            }
        },
        methods:{
            onReset(){
                this.result = [];
            },
            onClickScreen(){
                if(this.state === 'waiting'){
                    this.state = 'ready';
                    this.message = '초록색이 되면 클릭하세요.';
                    timeOut = setTimeout(()=>{
                        this.state = 'now';
                        this.message = '클릭'
                        startTime = new Date();
                    }, Math.floor(Math.random() * 1000) +2000);//2~3sec
                }else if(this.state === 'ready'){
                    clearTimeout(timeOut);
                    this.state = 'waiting';
                    this.message = '빠름'
                }else if(this.state === 'now'){
                    endTime = new Date();
                    this.state = 'waiting';
                    this.message = '클릭해서 시작';
                    this.result.push(endTime - startTime);
                }
            }
        }
    }
</script>

<style scoped> 
    #screen{
        width:300px;
        height:200px;
        text-align: center;
        user-select: nond;
    }

    #screen.waiting{
        background-color:aqua;
    }

    #screen.ready{
        background-color: pink;
        color:white;
    }

    #screen.now{
        background-color: greenyellow;
    }
</style>