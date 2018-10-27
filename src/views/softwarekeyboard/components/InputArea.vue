<template>
  <div class="home">
    <div>
      <label>Text Input 01</label>
      <input id="textField01" v-model="textField01" type="text" @blur="onBlurTextField" maxlength="10"/>
    </div>
    <div>
      <label>Text Input 02</label>
      <input id="textField02" v-model="textField02" type="password" @blur="onBlurTextField" maxlength="8"/>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch, Prop } from 'vue-property-decorator';
import SpecialKey from '@/views/softwarekeyboard/components/SpecialKey.ts';

@Component
export default class InputArea extends Vue {
  public textField01: string = '';
  public textField02: string = '';

  @Prop()
  public inputKey!: string;

  // events.

  public mounted() {
    this.focusById('userCode');
  }

  public onBlurTextField(event: FocusEvent) {
    this.restoreFocusWhenNotNextTextField(
      event.target as HTMLInputElement,
      event.relatedTarget as HTMLInputElement,
    );
  }

  @Watch('inputKey')
  public changeInputKey() {
    if (this.inputKey === '') return;
    this.insertCharactorForActiveInput(this.inputKey);
  }

  // methods.

  private insertCharactorForActiveInput(charactor: string) {
    const textField = this.activeTextField();
    if (this.whenSpecialCaractor(textField, charactor)) return;
    this.insertCharactor(textField, charactor);
  }

  private whenSpecialCaractor(textField: HTMLInputElement, key: string): boolean {
    const sKey = this.toSpecialKey(key);
    if (sKey === null) return false;
    if (sKey === SpecialKey.MoveLeft) this.moveCaret(textField, -1);
    if (sKey === SpecialKey.MoveRight) this.moveCaret(textField, 1);
    if (sKey === SpecialKey.Delete) this.deleteOneCharactorBeforeCaret(textField);
    if (sKey === SpecialKey.Destroy) textField.value = '';
    return true;
  }

  private insertCharactor(textField: HTMLInputElement, oneCharactor: string) {
    const nowText = textField.value;
    const nowPos = textField.selectionStart;
    if (nowPos === null) return;
    const selectEnd = textField.selectionEnd;
    const secondPos = selectEnd === null ? nowPos : (selectEnd as number);
    const maxLength = textField.maxLength;

    const lertPart = nowText.substr(0, nowPos);
    const rightPart = nowText.substr(secondPos);
    const newText = lertPart + oneCharactor + rightPart;
    if (newText.length > maxLength) return;

    this.syncInnerValue(textField, newText);

    const nextPosotion = nowPos + oneCharactor.length;
    textField.setSelectionRange(nextPosotion, nextPosotion);
   }

  private moveCaret(textField: HTMLInputElement, movePosition: number) {
    const startPos = textField.selectionStart;
    if (startPos === null) return;
    const movedPos = startPos + movePosition;
    if (movedPos < 0) return;
    textField.selectionStart = movedPos;
    textField.selectionEnd = movedPos;
  }

  private deleteOneCharactorBeforeCaret(textField: HTMLInputElement) {
    const nowText = textField.value;
    let startPos = textField.selectionStart;
    const endPos = textField.selectionEnd as number;
    if (startPos === null) return;
    if (startPos === endPos) startPos--;

    const newText = nowText.substr(0, startPos) + nowText.substr(endPos);
    this.syncInnerValue(textField, newText);

    textField.setSelectionRange(startPos, startPos);
  }

  private syncInnerValue(textField: HTMLInputElement, text: string) {
    const id = textField.id;
    textField.value = text;
    if (id === 'textField01') this.textField01 = text;
    if (id === 'textField02') this.textField02 = text;
  }

  private toSpecialKey(key: string): SpecialKey | null {
    const hitKeys = Object.values(SpecialKey)
      .filter((i) => i as string === key);
    if (hitKeys.length === 0) return null;
    return hitKeys[0];
  }

  private focusById(id: string) {
    const textField = document.getElementById(id);
    if (textField === null) return;
    textField.focus();
  }

  private isTextField(element: Element | null): boolean {
    if (element === null || element.tagName !== 'INPUT') return false;
    const type = (element as HTMLInputElement).type.toLowerCase();
    return type === 'text' || type === 'password';
  }

  private activeTextField(): HTMLInputElement {
    const elem = document.activeElement;
    if (!this.isTextField(elem)) return new HTMLInputElement(); // Dummy
    return elem as HTMLInputElement;
  }

  private restoreFocusWhenNotNextTextField(
    exitTextField: HTMLInputElement,
    nextElement: HTMLInputElement | null,
  ) {
    const type = nextElement === null ? '' : nextElement.type.toLowerCase();
    if (type === 'text' || type === 'password') return;
    setTimeout(() => exitTextField.focus(), 1); // フォーカス系イベント中でフォーカスを弄るとおかしくなるので、別スレッドで。
  }
}
</script>
