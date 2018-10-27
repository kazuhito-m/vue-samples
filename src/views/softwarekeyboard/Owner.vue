<template>
  <div class="home">
    <h1>SoftwareKeyboard and Structure Sample</h1>
    <InputArea :inputKey="actieveText" ref="inputArea"/>
    <Keyboard @input="keyPress"/>
    <p>
      <button @click="onClickGet">Get Value</button>
    </p>

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
export default class Owner extends Vue {
  public actieveText: string = '';

  // eveents.

  public keyPress(key: string) {
    this.actieveText = key;
    setTimeout(() => this.actieveText = '', 1); // 非同期でクリアすることにより、このイベント後すぐにクリアする。
  }

  public onClickGet() {
    const inputArea = this.$refs.inputArea as InputArea;
    alert('onClickGet\ninput01:' + inputArea.textField01 + '\ninput02:' + inputArea.textField02);
  }
}
</script>
