<template>
  <div>
    <!-- Тип тап -->
    <div
      :class="$style.info">
      <TipTapVue />
      <ul>
        <li>
          + Кастомизация/стилизация всего чего только можно.
        </li>
        <li>
          + Все расширения можно расширять, добавляя свой функционал.
        </li>
        <li>
          - Времязатратно
        </li>
        <li>
          - Есть платные прикольные фичи
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

import TipTapVue from './components/TipTapVue.vue';

export default defineComponent({
  name: 'App',
  components: {
    TipTapVue
  }
})

</script>

<style module>
.info {
  border: 1px solid black;
  padding: 10px;
}
</style>
