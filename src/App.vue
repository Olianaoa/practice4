<script setup>
import game_settings from './components/game_settings.vue'
import game from './components/game.vue'
import statistics from './components/statistics.vue'
import modal from './components/modal.vue'

import { reactive } from 'vue';

const local_data = reactive({
  you_here: 1,

  setting_diff: '',
  setting_target: '',
  setting_size: '',
  game_result: '',
  array_stat: [],

  game_status: false,

  modalShow: false,
  modalText: '',
  modalType: '',
})

function move_to_game() {
  local_data.modalShow = true;
  local_data.modalText = 'Вы не можете перейти на страницу с игрой, пока не выберите настройки для игры';
  local_data.you_here = 1;
}

function game_setting_app({ diff, target_num, size }) {
  local_data.setting_diff = diff
  local_data.setting_target = target_num
  local_data.setting_size = size
  local_data.you_here = 2;
}

function emit_result(result) {
  local_data.game_result = result;

  //уровень сложности словами (для вывода в таблицу)
  if (local_data.setting_diff === '1500') {
    local_data.setting_diff = 'Легкий';
  }
  if (local_data.setting_diff === '1000') {
    local_data.setting_diff = 'Средний';
  }
  if (local_data.setting_diff === '700') {
    local_data.setting_diff = 'Сложный';
  }

  local_data.array_stat.push({
    difficulty: local_data.setting_diff,
    targetNum: local_data.setting_target,
    targetSize: local_data.setting_size,
    result: local_data.game_result,
  });

  local_data.you_here = 3;
}

function game_status(game_status) {
  local_data.game_status = game_status
}

</script>

<template>
  <p>djfgidfhbkjfbkfb</p>
  <p>slkdjhfdufjhgkdfgjdkfljblkgcbmgclk</p>
  <p>отлоатиьсчитчоитлвтиьаитатичияоаиаоялвамт поитмьитслоитлоатлотиолстотмдыявь смовмтлвоамтлвомтловтмлоымвтловмтловяамтлоястмлсотмс</p>
  <div class="buttons">
    <button @click="local_data.you_here = 1" :disabled="local_data.game_status"
      :class="{ you_here: local_data.you_here === 1 }">Настройки</button>
    <button @click="move_to_game" :disabled="local_data.game_status"
      :class="{ you_here: local_data.you_here === 2 }">Игра</button>
    <button @click="local_data.you_here = 3" :disabled="local_data.game_status"
      :class="{ you_here: local_data.you_here === 3 }">Статистика</button>
  </div>

  <game_settings v-if="local_data.you_here === 1" @game_settings_emit="game_setting_app" />

  <game v-if="local_data.you_here === 2" 
  :setting_diff=local_data.setting_diff 
  :setting_target=local_data.setting_target
  :setting_size=local_data.setting_size 
  @gameEmit="emit_result" 
  @gameStatus="game_status" />

  <statistics v-if="local_data.you_here === 3" 
  :array="local_data.array_stat" />


  <div>
    <modal v-if="local_data.modalShow" modalType="yes" 
    :modalText="local_data.modalText"
    @modalYes="local_data.modalShow = false" />
  </div>

</template>

<style scoped>
.buttons {
  padding: 20px;
  justify-content: space-around;
  display: flex;
  min-width: 1280px;
}

.you_here {
  border: double 5px rgb(0, 198, 132);
}
</style>
