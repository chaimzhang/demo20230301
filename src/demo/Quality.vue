<template>
  <a-button id="quality" ref="quality" @keyup="onKeyup" @focusout="emitVal" @focus="$emit('focus')">
    <a-tag color="blue" @click="submit('1')"
           v-if="selected!=='0'">
      合格
    </a-tag>
    <a-tag v-if="selected!=='1'"
           @click="submit('0')"
           color="blue"
           :class="{'selected':selected==='0'}">
      不合格
    </a-tag>
  </a-button>
</template>

<script>
import {KEYBOARD} from "@/demo/Keycode";

/**
 *  + 合格
 *  - 不合格
 *  Backspace 还原
 *  Enter、上下左右键 确定，并移动焦点
 */
export default {
  name: "Quality",
  model: {
    prop: 'value',
    event: 'change'
  },
  props: {
    value: String
  },
  data() {
    return {
      selected: ''
    }
  },
  watch: {
    value: {
      handler(newVal) {
        this.selected = newVal;
      },
      immediate: true
    }
  },
  mounted() {
  },
  methods: {
    onKeyup({key}) {
      switch (key) {
        case KEYBOARD.Plus:
          this.selected = '1';
          break;
        case KEYBOARD.Minus:
          this.selected = '0';
          break;
        case KEYBOARD.Backspace:
          this.selected = '';
          break;
        default:
          break;
      }
    },
    submit(val) {
      this.selected = val;
    },
    emitVal() {
      this.$emit('change', this.selected)
    },
    focus() {
      this.$refs.quality.$el.focus();
    }
  }
}
</script>

<style scoped lang="less">

#quality {
  width: 100%;
  display: flex;
  align-content: center;
  align-items: center;
  background: transparent;

  &:not(:focus) {
    border: 0;
  }

  > span:not(:first-child) {
    margin-left: 15px
  }

  /deep/ .selected {
    color: red;
  }
}
</style>