<template>
  <div id="demo1" @keydown="keyDown">
    <a-table :data-source="tableData">
      <a-table-column title="图位">1</a-table-column>
      <a-table-column title="公称值">2</a-table-column>
      <a-table-column width="15%" title="检测值1#" data-index="j0">
        <template slot-scope="data,record,index">
          <template v-if="index===0">
            <a-input :ref="index+'0'" v-model="record['j0']" @focus="onfocus(index,0)"/>
          </template>
          <template v-else>
            <quality :ref="index+'0'" v-model="record['j0']" @focus="onfocus(index,0)"/>
          </template>
        </template>
      </a-table-column>
      <a-table-column title="偏差值1#">-</a-table-column>
      <a-table-column width="15%" title="检测值2#" data-index="j1">
        <template slot-scope="data,record,index">
          <template v-if="index===0">
            <a-input :ref="index+'1'" v-model="record['j1']" @focus="onfocus(index,1)"/>
          </template>
          <template v-else>
            <quality :ref="index+'1'" v-model="record['j1']" @focus="onfocus(index,1)"/>
          </template>
        </template>
      </a-table-column>
      <a-table-column title="偏差值2#">-</a-table-column>
      <a-table-column width="15%" title="检测值3#" data-index="j2">
        <template slot-scope="data,record,index">
          <template v-if="index===0">
            <a-input :ref="index+'2'" v-model="record['j2']" @focus="onfocus(index,2)"/>
          </template>
          <template v-else>
            <quality :ref="index+'2'" v-model="record['j2']" @focus="onfocus(index,2)"/>
          </template>
        </template>
      </a-table-column>
      <a-table-column title="偏差值3#">-</a-table-column>
    </a-table>
  </div>
</template>
<script>
import Quality from "@/demo/Quality";
import {KEYBOARD} from "@/demo/Keycode";

export default {
  components: {Quality},
  data() {
    return {
      focusPosition: {
        row: 0,
        col: 0
      },
      colMax: 2,
      tableData: [
        {
          key: '0',
          j1: '1',
          j2: '',
        },
        {
          key: '1',
          j1: '0',
          j2: '',
        },
        {
          key: '2',
          j1: '',
          j2: '',
        },
      ]
    }
  },

  mounted() {
    this.focus();
  },
  methods: {
    keyDown({key}) {
      switch (key) {
        case KEYBOARD.Up:
          if (this.focusPosition.row > 0) {
            this.focusPosition.row--;
          }
          break;
        case KEYBOARD.Down:
          if (this.focusPosition.row < this.tableData.length - 1) {
            this.focusPosition.row++;
          }
          break;
        case KEYBOARD.Left:
          if (this.focusPosition.col > 0) {
            this.focusPosition.col--;
          } else if (this.focusPosition.row > 0) {
            this.focusPosition.row--;
            this.focusPosition.col = this.colMax;
          }
          break;
        case KEYBOARD.Right:
          if (this.focusPosition.col < this.colMax) {
            this.focusPosition.col++;
          } else if (this.focusPosition.row < this.tableData.length - 1) {
            this.focusPosition.row++;
            this.focusPosition.col = 0;
          }
          break;
        case KEYBOARD.Enter:
          this.keyDown({key: KEYBOARD.Right})
          break;
        default:
          break;
      }
      this.focus();
    },
    focus() {
      this.$refs[this.focusPosition.row.toString(10) + this.focusPosition.col.toString(10)].focus();
    },
    onfocus(row, col) {
      this.focusPosition = {row, col};
    }
  }
}
</script>
<style lang="less">

</style>
