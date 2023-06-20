<template>
    <div class="ball" :style="{background}">{{number}}</div>
</template>

<script>
    export default {
        name: 'LottoBall',
        props:['number'],
        data(){
            let background;
            return{
                background,
            }
        }
    }
</script>

<style>
    .ball{
        display: inline-block;
        background-color: pink;
        border-radius: 20px;
        width:40px;
        height:40px;
        line-height: 40px;
        font-size: 20px;
        text-align: center;
        margin-left: 20px;
    }
</style>