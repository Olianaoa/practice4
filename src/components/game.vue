<script setup>
import modal from './modal.vue';
import { reactive } from 'vue';

const props = defineProps({
    setting_diff: null,
    setting_target: null,
    setting_size: null,
})

const local = reactive({
    modalShow: false,
    modalText: '',
    modalType: '',

    targets: [],
    result: 30,
    targets_movements: null,
    catched: 0,
    started: false,
    timer_for_result: 30,
    game_status: false,
})

function catching(index) {
    local.targets[index].display = 'none'
    local.result += 3
    local.catched += 1
    if (local.catched == props.setting_target) {
        finish_game('Вы выиграли! Параметры игры можно посмотреть на странице `Статистика`. Результат: ') 
    }
}

function startRace() {
    if (!local.started) {
        //запуск игры 
        local.started = true;
        emit('gameStatus', local.started); // отправляю статус игры, чтобы заблокировать кнопки (:disabled=true)

        for (let index = 0; index < props.setting_target; index++) { //создание кол-ва целей по значению из настроек
            local.targets.push({//добавление в массив +цели становятся видимыми
                display: 'block',
                transform: `translateX(${Math.random() * 500}px) translateY(${Math.random() * 500}px) `
            })
        };
        //изменение положения целей на поле
        local.targets_movements = setInterval(changePos, props.setting_diff); // положение меняется каждые props.setting_diff секунд

        //запуск таймера (меняет результат)
        local.timer_for_result = setInterval(() => {
            if (local.result > 0 && local.targets.length !== 0) {
                local.result -= 1; // таймер уменьшается каждую секунду, если result не 0 и все еще есть цели на поле
            }
            if (local.result === 0 && local.targets.length > 0) { // если время истекло, но цели на поле остались
                finish_game('Вы проиграли :( Результат: ')
            }
            
        }, 1000);//все это происходит за секунду 
    }

}

function changePos() { // функция на изменение положения целей
    local.targets.forEach(element => {
        element.transform = `translateX(${Math.random() * 500}px) translateY(${Math.random() * 500}px) `
    })
}
function give_up_btn() {
    local.result = 0;
    finish_game('Вы сдались :( Результат: ')
}

const emit = defineEmits([
    'gameEmit', 'gameStatus'
])

function send_result() {
    emit('gameEmit', local.result)
    local.modalShow = false
}

function finish_game(text) {
    clearInterval(local.timer_for_result);
    clearInterval(local.targets_movements);
    local.modalText = text;
    local.modalShow = true;// модальное окно с результатом (поражение)
    local.started = false;
    emit('gameStatus', local.started);// отправляю статус игры, чтобы разблокировать кнопки (:disabled=false)
    local.targets = [];
}
</script>

<template>
    <h1>Осталось времени: {{ local.result }}</h1>
    <!-- v-if="!local.started" - убираю кнопку с началом игры
        v-if="local.started" - показываю кнопку -->
    <button @click="startRace" v-if="!local.started">Начать игру</button>
    <button @click="give_up_btn" v-if="local.started">Сдаться</button>

    <div class="game-field">
        <div v-for="(target, index) in local.targets"  class="target"
            :style="{ display: target.display, transform: target.transform, width: props.setting_size, height: props.setting_size }"
            @click="catching(index)">
        </div>
    </div>

    <div>
        <modal v-if="local.modalShow" modalType="yes" 
        :modalText="local.modalText" 
        :ModalTestResult="local.result"
            @modalYes="send_result" />
    </div>

</template>

<style scoped>
.game-field {
    position: relative;
    width: 600px;
    height: 600px;
    border: 2px solid black;
    margin: 20px auto;
}

.target {
    position: absolute;
    background-color: rgb(255, 170, 187);
    border: dotted 3px rgb(0, 198, 132);
    border-radius: 50%;
    cursor: pointer;
    transition: 1s;
}
</style>