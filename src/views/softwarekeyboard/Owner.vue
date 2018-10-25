<template>
  <div class="home">
    <h1>SoftwareKeyboard and Structure Sample</h1>
    <InputArea/>
    <Keyboard/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import InputArea from '@/views/softwarekeyboard/components/InputArea.vue';
import Keyboard from '@/views/softwarekeyboard/components/Keyboard.vue';

@Component({
  components: {
    InputArea,
    Keyboard,
  },
})
export default class Owner extends Vue {}
</script>
