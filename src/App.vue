<template>
  <div>
    <TipTapVue />
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

