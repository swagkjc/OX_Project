<template>
  <div>
    <table>
      <tr v-for="(line, lineId) in table" :key="lineId">
        <td v-for="(item, itemId) in line" :key="itemId">
          <OXCell
            :player="item"
            :row="lineId"
            :col="itemId"
            @cell-click="cellclick"
          />
        </td>
      </tr>
    </table>
  </div>
</template>
<script>
import OXCell from './OXCell.vue'
export default {
  data () {
    return {
      turn: 'O',
      table: [
        ['X', '-', '-'],
        ['-', '-', '-'],
        ['-', '-', '-']
      ]
    }
  },
  methods: {
    cellclick (row, col) {
      console.log('cell click' + row + ' ' + col)
      this.table[row][col] = this.turn
    }
  },
  components: {
    OXCell
  }
}
</script>
<style></style>
