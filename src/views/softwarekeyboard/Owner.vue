<template>
  <div class="home">
    <h1>SoftwareKeyboard and Structure Sample</h1>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import InputArea from '@/views/softwarekeyboard/InputArea.vue'
import Keyboard from '@/views/softwarekeyboard/Keyboard.vue'

@Component({
  InputArea,
  Keyboard,
})
export default class Owner extends Vue {}
</script>
