<script setup>

import { onMounted, reactive, onUnmounted } from 'vue';
const local = reactive({
    diff: '',
    num_target: '',
    size_target: '',
    error: '',
})
// vue
const emit = defineEmits([
    'game_settings_emit'
])
function game_settings_saved() {
    if (local.diff == '' || local.num_target == '' || local.size_target == '') {
        local.error = " Ошибка! Необходимо выбрать все параметры"
    }
    else {
        local.error = ""
        emit('game_settings_emit', {
            diff: local.diff,
            target_num: local.num_target,
            size: local.size_target
        })
    }
}

onMounted(() => {
    console.log('alive')
})
onUnmounted(() => {
    console.log('dead')
})

</script>

<template>
    <h2>Страница с настройками игры</h2>
    <p>Выберите уровень сложности, количество целей, размер целей</p>
    <div class="all_settings">
        <div class="diff_level">
            <fieldset>
                <!-- value - скорость перемещения кнопок -->
                <legend>Уровень сложности</legend>
                <input type="radio" name="diff" id="diff_1" v-model="local.diff" value="1500"><label
                    for="diff_1">Легкий</label><br>
                <input type="radio" name="diff" id="diff_2" v-model="local.diff" value="1000"><label
                    for="diff_2">Средний</label><br>
                <input type="radio" name="diff" id="diff_3" v-model="local.diff" value="700"><label
                    for="diff_3">Сложный</label><br>
            </fieldset>
        </div>
        <div class="num_target">
            <fieldset>
                <legend>Количество целей</legend>
                <input type="radio" name="target" id="target_1" v-model="local.num_target" value="10"><label
                    for="target_1">10</label><br>
                <input type="radio" name="target" id="target_2" v-model="local.num_target" value="15"><label
                    for="target_2">15</label><br>
                <input type="radio" name="target" id="target_3" v-model="local.num_target" value="20"><label
                    for="target_3">20</label><br>
            </fieldset>
        </div>
        <div class="size_target">
            <fieldset>
                <!-- width -  -->
                <legend>Размер целей</legend>
                <input type="radio" name="size" id="size_1" v-model="local.size_target" value="30px"><label
                    for="size_1">30px</label><br>
                <input type="radio" name="size" id="size_2" v-model="local.size_target" value="50px"><label
                    for="size_2">50px</label><br>
                <input type="radio" name="size" id="size_3" v-model="local.size_target" value="80px"><label
                    for="size_3">80px</label><br>
            </fieldset>
        </div>
    </div>

    <button @click="game_settings_saved()">Сохранить параметры</button>
    <div class="error">{{ local.error }}</div>
</template>

<style scoped>
fieldset {
    text-align: left;
    width: 200px;
}

.all_settings {
    margin-top: 10px;
    display: flex;
    justify-content: center;
    gap: 10px;
}

button {
    margin: 20px;
}

.error {
    color: red;
}
</style>