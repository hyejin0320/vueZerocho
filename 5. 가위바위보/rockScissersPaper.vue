<template>
    <div>
        <div id="computer" :style='computedStyleObj'></div>
        <div>
            <button @click="onClickMehtod('water')">물</button>
            <button @click="onClickMehtod('fire')">불</button>
            <button @click="onClickMehtod('grass')">풀</button>
        </div>
        <div>{{result}}</div>
        <div>점수 {{score}}점</div>
        <!-- <lifecylce-example v-if="true"/> -->
    </div>
</template>

<script>
let intalval = null;
const rspCode = {
    water: '-222px',
    fire: '-111px',
    grass: '0'
}

const scores = {
    water : 0,
    fire : 1,
    grass : -1
}

const computerChoice = (imageCode) =>{
    return Object.entries(rspCode).find((v)=>{
        return v[1] === imageCode;
    })[0];
}

let interval = null;

  export default{
    data(){
        return{
            result:'',
            score: 0,
            imageCode: rspCode.fire,
        }
    },
    computed:{
        computedStyleObj(){
            return{
                background: `url(https://cdn.wikimg.net/en/splatoonwiki/images/thumb/0/09/S3_Splatfest_Grass_vs_Fire_vs_Water.png/334px-S3_Splatfest_Grass_vs_Fire_vs_Water.png) ${this.imageCode} 0 no-repeat`
            }
        }
    },
    methods:{

        changeAttr(){
            interval = setInterval(() => {
                if(this.imageCode === rspCode.fire){
                    this.imageCode = rspCode.water
                }else if(this.imageCode === rspCode.water){
                    this.imageCode = rspCode.grass;
                }else if(this.imageCode === rspCode.grass){
                    this.imageCode = rspCode.fire;
                }
            }, 100);
        },

        onClickMehtod(choice){
            clearInterval(interval);
            const myScore = scores[choice];
            const cpuScore = scores[computerChoice(this.imageCode)];
            const diff = myScore - cpuScore;

            if(diff === 0){
                this.result = '무승부';
            }else if([-1,2].includes(diff)){
                this.result = '승리';
                this.score += 1;
            }else{
                this.result = '패배';
                this.score -= 1;
            }

            setTimeout(()=>{
                this.changeAttr();
            }, 1500)
        }
    },
    created(){
        // console.log('created');
    },
    mounted(){
        // console.log('mounted');
        this.changeAttr();
    },
    updated(){
        // console.log('updated');
    },
    beforeDestroy() {
        // console.log(beforeDestroy);
        clearInterval(interval);
    },
    destroyed() {
        // console.log('destroyed');
    },
  }
</script>

<style scoped> 
    #computer{
        width:111px;
        height: 200px;
        background-position: 0 0;
    }
</style>