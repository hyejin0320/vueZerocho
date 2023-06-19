<template>
    <div>
        <div>{{result}}</div>
        <form @submit.prevent="onSubmitForm">
            <input ref="answer" minlength="4" maxlenght="4" v-model="value">
            <button type="submit">입력</button>
        </form>
        <div>시도 : {{tries.length}}</div>
        <ul>
            <li v-for="t in tries">
                <div>{{t.try}}</div>
                <div>{{t.result}}</div>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                answer: setRandNumber(),
                value: '',
                result: '',
                tries: [],
            }
        },
        methods:{
            onSubmitForm(e){
                if(this.value === this.answer.join('')){
                    this.tries.push({
                        try: this.value,
                        result: '홈런'
                    });
                
                    this.result = '홈런';
                    this.tries = [];
                }else{
                    if(this.tries.length >= 9){
                        this.result = `시도 횟수 10번 초과 / ${this.answer.join(',')}`;
                        this.answer = setRandNumber();
                        this.tries = [];
                    }
                    let strike = 0;
                    let ball = 0;
                    const answerArr = this.value.split('').map(v => parseInt(v));
                    for( let i=0;i<4;i++){
                        if(answerArr[i] === this.answer[i]){ //strike
                            strike++;
                        }else if(this.answer.includes(answerArr[i])){//ball
                            ball++;
                        }
                    }
                    this.tries.push({
                        try: this.value,
                        result: `정답: ${this.answer} / 스트라이크: ${strike} / 볼: ${ball} `
                    })
                }

                this.value = '';
                this.$refs.answer.focus();
            }
        }
    };

    const setRandNumber = () => {
        const candidates = [1,2,3,4,5,6,7,8,9];
        const arr = [];
        for(let i=0;i<4;i++){
            const chosen = candidates.splice(Math.floor(Math.random() * (9-i)), 1)[0];
            arr.push(chosen);
        }

        return arr;
    }
</script>

<style>
</style>