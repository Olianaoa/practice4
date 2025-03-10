<script setup>
import { reactive } from 'vue';
const props = defineProps({
    array: Array,
})

const local = reactive({
    diff: '',
    num_target: '',
    size_target: '',
})
</script>

<template>
    <h2>Таблица с результатами прохождения теста</h2>
    <div class="all_things">
        <div>
            <div class="in_line">
                <div class="elem">Уровень сложности </div>
                <div class="elem">Кол-во целей</div>
                <div class="elem">Размер целей</div>
                <div class="elem">Результат</div>
            </div>
            <div v-for="elem in props.array" class="in_line">
                <template class="in_line" v-if="(!local.diff || elem.difficulty == local.diff)
                    && (!local.num_target || elem.targetNum == local.num_target)
                    && (!local.size_target || elem.targetSize == local.size_target)">
                    <div class="elem"> {{ elem.difficulty }} </div>
                    <div class="elem"> {{ elem.targetNum }} </div>
                    <div class="elem"> {{ elem.targetSize }} </div>
                    <div class="elem"> {{ elem.result }} </div>
                </template>
            </div>
        </div>
        <!-- влоарпавпровамиволамт
         ывмоавоиолитлоястияса
         очвмсилчяостмлчстм -->
        <div class="settings">
            <!--Сложность -->
            <fieldset>
                <legend>Фильтр сложности</legend>
                <input type="radio" name="diff" id="diff_1" v-model="local.diff" value="Легкий"><label
                    for="diff_1">Легкий</label><br>
                <input type="radio" name="diff" id="diff_2" v-model="local.diff" value="Средний"><label
                    for="diff_2">Средний</label><br>
                <input type="radio" name="diff" id="diff_3" v-model="local.diff" value="Сложный"><label
                    for="diff_3">Сложный</label><br>
                <input type="radio" name="diff" id="no_diff" checked v-model="local.diff" value=""><label
                    for="no_diff">Нет
                    сортировки</label><br>
            </fieldset>
            <!--Цели -->
            <fieldset>
                <legend>Фильтр кол-ва целей</legend>
                <input type="radio" name="target" id="target_1" v-model="local.num_target" value="10"><label
                    for="target_1">10</label><br>
                <input type="radio" name="target" id="target_2" v-model="local.num_target" value="15"><label
                    for="target_2">15</label><br>
                <input type="radio" name="target" id="target_3" v-model="local.num_target" value="20"><label
                    for="target_3">20</label><br>
                <input type="radio" name="target" id="no_target" v-model="local.num_target" value="" checked><label
                    for="no_target">Нет
                    сортировки</label><br>
            </fieldset>
            <!-- Размер -  -->
            <fieldset>
                <legend>Фильтр размера</legend>
                <input type="radio" name="size" id="size_1" v-model="local.size_target" value="30px"><label
                    for="size_1">30px</label><br>
                <input type="radio" name="size" id="size_2" v-model="local.size_target" value="50px"><label
                    for="size_2">50px</label><br>
                <input type="radio" name="size" id="size_3" v-model="local.size_target" value="80px"><label
                    for="size_3">80px</label><br>
                <input type="radio" name="size" id="no_size" v-model="local.size_target" value="" checked><label
                    for="no_size">Нет сортировки</label><br>
            </fieldset>
        </div>
    </div>
</template>

<style scoped>
.all_things {
    display: flex;
    justify-content: space-around;
}
fieldset {
    text-align: left;
    width: 200px;
}
.elem {
    width: 200px;
    height: 50px;
    border: 1px solid grey;
}
.in_line {
    display: flex;
}
</style>