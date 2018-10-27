<template>
  <div id="softKeyboardArea">
    <div v-for="key in inputableCharactors" :key="key.key">
      <button class="baseKey key" @click="onClickCharacter(key)">{{key}}</button>
    </div>
    <div class="clearfix"></div>
    <div>
      <button class="baseKey symbolKey">?</button>
      <button class="baseKey changeKey">A/a</button>
      <button class="baseKey spaceKey" @click="onClickCharacter(' ')"></button>
      <button class="baseKey moveKey" @click="onClickMoveLeft"><span class="CaretBackArrow"/></button>
      <button class="baseKey moveKey" @click="onClickMoveRight"><span class="CaretNextArrow"/></button>
      <button class="baseKey deleteKey" @click="onClickDelete">Delete</button>
      <button class="baseKey deleteKey" @click="onClickDestroy">Destroy</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Emit } from 'vue-property-decorator';
import SpecialKey from '@/views/softwarekeyboard/components/SpecialKey.ts';

@Component
export default class Keyboard extends Vue {
  private static readonly ALPHNUM = '1234567890QWERTYUIOPASDFGHJKL@ZXCVBNM.-_';

  // events.

  public onClickCharacter(key: string) {
    this.input(key);
  }

  public onClickMoveLeft() {
    this.input(SpecialKey.MoveLeft);
  }

  public onClickMoveRight() {
    this.input(SpecialKey.MoveRight);
  }

  public onClickDelete() {
    this.input(SpecialKey.Delete);
  }

  public onClickDestroy() {
    this.input(SpecialKey.Destroy);
  }

  @Emit()
  public input(key: string) {
    // dymmy funciton.
  }

  // prpoerties.

  private get inputableCharactors() {
    return Keyboard.ALPHNUM.split('');
  }
}
</script>

<style lang="scss" scoped>
$grayBtn: #dee0e2;
$pressedBtn: #bebebe;
#softKeyboardArea {
  // margin-left: 0px;
  width: 700px;
  align: center;

  .baseKey {
    height: 53px;
    float: left;
    border: none;
    border-radius: 6px 6px 8px 8px;
    margin: 4px;
    margin-bottom: 5px;
    background-color: $grayBtn;
    box-shadow: 0 3px 4px 1px gray;
    font-family: 'sans-serif';
    text-align: center;

    &:active {
      background-color: $pressedBtn;
      box-shadow: none;
    }
  }
  .clearKey {
    float: left;
    margin: 4px;
  }
  .key {
    width: 60px;
    font-size: 36px;
  }
  .moveKey {
    width: 60px;
    font-size: 36px;

    &:active {
      padding-top: 3px;
      padding-left: 10px;
    }
  }
  .symbolKey {
    width: 60px;

    font-size: 16px;
  }
  .changeKey {
    width: 60px;
    font-size: 28px;
  }
  .spaceKey {
    width: 196px;
  }
  .deleteKey {
    width: 94px;
    font-size: 18px;
  }
}

.CaretBackArrow,
.CaretNextArrow {
  display: inline-block;
  width: 14px;
  height: 14px;
  border-top: 4px solid #000;
  border-right: 4px solid #000;
}
.CaretBackArrow {
  margin-left: 7px;
  margin-bottom: 3px;
  -webkit-transform: rotate(-135deg);
  transform: rotate(-135deg);
}

.CaretNextArrow {
  margin-right: 7px;
  margin-bottom: 3px;
  -webkit-transform: rotate(45deg);
  transform: rotate(45deg);
}
</style>
