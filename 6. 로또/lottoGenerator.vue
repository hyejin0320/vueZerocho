<template>
    <div>
        <div>당첨 숫자</div>
        <div id="resultScreen">
            <lotto-ball v-for="ball in winBalls" :key="ball" v-bind:number="ball"></lotto-ball>
        </div>
        <br/>
        <div>보너스</div>
        <lotto-ball v-if="bonus" :number="bonus"></lotto-ball>
        <button v-if="redo" @click="onClickRedo">재추첨</button>
    </div>
</template>

<script>
    import lottoBall  from './lottoBall';

    const getWinNumbers = () =>{
        const candidate = Array(45).fill().map((v, i) => i+1);
        const shuffle = [];
        while(candidate.length > 0){
            shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]);
        }
        const bonusNumber = shuffle[shuffle.length-1];
        const winNumbers = shuffle.slice(0, 6).sort((p, c)=> p-c);
        return [...winNumbers, bonusNumber];
    };

    export default {
        components:{
            lottoBall
        },
        data(){
            return{
                winNumbers: getWinNumbers(),
                winBalls: [],
                bonus: null,
                redo: false,
            }
        },
        methods:{
            onClickRedo(){
                this.winNumbers = getWinNumbers();
                this.winBalls = [];
                this.bonus = null;
                this.redo = false;
                this.showBalls();
            },
            showBalls(){
                for(let i=0;i<this.winNumbers.length-1; i++){
                    setTimeout(()=>{
                        console.log(i)
                        console.log(i+' : '+this.winNumbers[i]);
                        this.winBalls.push(this.winNumbers[i]);
                    }, (i+1)*1000);
                }

                setTimeout(()=>{
                    this.bonus = this.winNumbers[6];
                    this.redo = true;
                }, 7000);
            }
        },
        computed:{

        },
        mounted(){
           this.showBalls();
        }
    };
</script>

<style scoped> 
</style>