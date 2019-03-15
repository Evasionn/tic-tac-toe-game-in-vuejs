<template>
  <div>
    <div class="header">
      <h1>Tic-Tac-Toe</h1>
    </div>
    <div class="game-board">
      <table>
        <tr v-for="(row, rowIndex) in game" :key="rowIndex">
          <td v-for="(column, columnIndex) in row" :key="columnIndex" v-on:click="click(rowIndex, columnIndex)">
            <transition name="fade">
              <span v-if="game[rowIndex][columnIndex]"> {{ game[rowIndex][columnIndex] }} </span>
            </transition>
          </td>
        </tr>
      </table>
       <div v-if="winner">
        <p>
          Game is over!
          <br />
          {{ winner }}
        </p>
       </div>
       <button class="button-restart" v-if="countEmptyCells() !== 9" v-on:click="reset">RESTART</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Game',
  methods: {
    isDone: function isDone() {
      // vertical checking
      for (let i = 0; i < 3; i++){
        if (this.game[0][i] && this.game[0][i] === this.game[1][i] && this.game[0][i] === this.game[2][i]){
          return true;
        }
      }

      // horizantal checking
      for (let i = 0; i < 3; i++){
        if (this.game[i][0] && this.game[i][0] === this.game[i][1] && this.game[i][0] === this.game[i][2]){
          return true;
        }
      }

      // diagonal checking
      if (this.game[0][0] && this.game[0][0] === this.game[1][1] && this.game[0][0] === this.game[2][2]){
        return true;
      }
      if (this.game[0][2] && this.game[0][2] === this.game[1][1] && this.game[0][2] === this.game[2][0]){
        return true;
      }

      return false;
    },
    countEmptyCells: function countEmptyCells(){
      // checking all fields is full
      let emptyCount = 0;
      for (let i = 0; i < 3; i++){
        for (let j = 0; j < 3; j++){
          if (!this.game[i][j]){
            emptyCount++;
          }
        }
      }

      return emptyCount;
    },
    reset: function reset(){
      this.game = [['', '', ''], ['', '', ''], ['', '', '']];
      this.winner = '';
      this.turn = 'X';
    },
    computerMove: function computerMove(){
      for (;;){
        const randomX = Math.floor(Math.random() * 3);
        const randomY = Math.floor(Math.random() * 3);
        if (!this.game[randomX][randomY]){
          this.game[randomX][randomY] = 'O';
          break;
        }
      }
    },
    click: function click(x, y) {
      if(this.winner || this.game[x][y] || this.turn === 'O'){
        return;
      }

      this.game[x][y] = 'X';
      this.turn = 'O';

      if (this.isDone()) {
        this.winner = 'X Winner!';
      } else if (this.countEmptyCells() === 0){
        this.winner = 'XO Draw!';
      } else{
        setTimeout(() => {
          this.computerMove();
          if (this.isDone()) {
            this.winner = 'O Winner!';
          }
          this.turn = 'X';
          this.$forceUpdate();
        }, 500);
      }
      this.$forceUpdate();
    }
  },
  data() {
    return {
      turn: 'X',
      winner: '',
      game: [
        ['', '', ''],
        ['', '', ''],
        ['', '', ''],
      ]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.game-board table {
  margin: 0 auto;
  border-collapse: collapse;
}
.game-board tr:first-child,
.game-board tr:nth-child(2) {
  border-bottom: 1px solid #ccc;
}
.game-board tr td:first-child,
.game-board tr td:nth-child(2) {
  border-right: 1px solid #ccc;
}
.game-board td {
  font-size: 30px;
  width: 60px;
  height: 60px;
  line-height: 60px;
  text-align: center;
}
.button-restart {
  border: none;
  background-color: #7899d4;
  padding: 10px;
  margin-top: 20px;
  color: #2c3e50;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
