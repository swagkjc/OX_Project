<template>
  <div>
    <div v-if="finish">
      <!-- <div v-if="winner !== '-'">{{ winner }} Winn !!!</div>
      <div v-else>Draw !!!</div> -->
      <span>{{ result }}</span>
      <button @click="newGame">New Game</button>
    </div>
    <div v-else>{{ turn }} Turn</div>

    <table>
      <tr v-for="(line, lineId) in table" :key="lineId">
        <td v-for="(item, itemId) in line" :key="itemId">
          <OXCell
            :player="item"
            :row="lineId"
            :col="itemId"
            :grade="gradeTable[lineId][itemId]"
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
      count: 0,
      finish: false,
      winner: '-',
      turn: 'O',
      table: [
        ['-', '-', '-'],
        ['-', '-', '-'],
        ['-', '-', '-']
      ],
      gradeTable: [
        [false, false, false],
        [false, false, false],
        [false, false, false]
      ]
    }
  },
  computed: {
    result () {
      if (this.finish && this.winner === '-') {
        return 'Draw!!!'
      } else {
        return this.turn + 'Win!!!'
      }
    }
  },
  methods: {
    newGame () {
      this.count = 0
      this.finish = false
      this.winner = '-'
      this.initTable()
      this.randomPlayer()
    },
    initTable () {
      for (let row = 0; row < 3; row++) {
        for (let col = 0; col < 3; col++) {
          this.table[row][col] = '-'
        }
      }
    },
    cellclick (row, col) {
      if (this.finish) return
      console.log('cell click' + row + ' ' + col)
      this.table[row][col] = this.turn
      this.count++
      if (this.checkWin(row, col)) {
        console.log('Win')
        this.finish = true
        this.winner = this.turn
        return
      }
      if (this.count === 9) {
        this.finish = true
        return
      }
      this.switchTurn()
    },
    switchTurn () {
      if (this.turn === 'O') {
        this.turn = 'X'
        return
      }
      this.turn = 'O'
    },
    randomPlayer () {
      if (Math.floor(100 * Math.random()) % 2 === 0) {
        this.turn = 'O'
      } else {
        this.turn = 'X'
      }
    },
    checkWin (row, col) {
      if (
        this.checkRow(row) ||
        this.checkCol(col) ||
        this.checkX1() ||
        this.checkX2()
      ) {
        return true
      }
    },
    checkRow (row) {
      for (let col = 0; col < 3; col++) {
        if (this.table[row][col] !== this.turn) {
          return false
        }
      }
      for (let col = 0; col < 3; col++) {
        this.gradeTable[row][col] = true
      }
      return true
    },
    checkCol (col) {
      for (let row = 0; row < 3; row++) {
        if (this.table[row][col] !== this.turn) {
          return false
        }
      }
      for (let row = 0; row < 3; row++) {
        this.gradeTable[row][col] = true
      }
      return true
    },
    checkX1 () {
      for (let index = 0; index < 3; index++) {
        if (this.table[index][index] !== this.turn) {
          return false
        }
      }
      for (let index = 0; index < 3; index++) {
        this.gradeTable[index][index] = true
      }
      return true
    },
    checkX2 () {
      for (let index = 0; index < 3; index++) {
        if (this.table[index][2 - index] !== this.turn) {
          return false
        }
      }
      for (let index = 0; index < 3; index++) {
        this.gradeTable[index][2 - index] = true
      }
      return true
    }
  },
  components: {
    OXCell
  },
  mounted () {
    this.randomPlayer()
  }
}
</script>
<style>
table {
  margin-left: auto;
  margin-right: auto;
  border: 1pt solid black;
}
td {
  text-align: center;
  vertical-align: middle;
  width: 50pt;
  height: 50pt;
}
</style>
