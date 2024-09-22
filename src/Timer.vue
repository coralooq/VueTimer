<script setup>
    import {ref, watchEffect} from 'vue'

    let sec = ref(15);
    let currentSec = ref(0);
    let procent = ref();
    let oldTimerId;

    function counting() {
        if(oldTimerId) clearInterval(oldTimerId);
        currentSec.value = 0;
        let oldTime = sec.value;
        let timerId = setInterval(() => {
            if(sec.value == 0 && currentSec.value == 0) {
                procent.value = 100;
                return;
            } 
            if(+sec.value != oldTime && procent.value > 99) clearInterval(timerId);
            currentSec.value += 0.01;
            procent.value = currentSec.value / +sec.value;
            if(+currentSec.value.toFixed(1) == +sec.value || +currentSec.value.toFixed(1) > +sec.value) {
                clearInterval(timerId);
            };
            oldTimerId = timerId;
        } , 10);
        
    };

    counting();
</script>

<template>
    <label for="pastTime">Прошедшее время: </label><progress id="pastTime" :value="procent"></progress><span>{{ currentSec.toFixed(1) }}s</span><br>
    <label for="duringTime">Длительность: </label><input @input="changeSec" type="range" id="duringTime" min="0" max="30" step="0.1" v-model="sec"></input><span>{{ sec }}s</span><br>
    <button @click="counting">Сброс</button>
</template>

<style>

</style>