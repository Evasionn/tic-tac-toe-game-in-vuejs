<template>
  <div>
    <div class="header">
      <h1>Tic-Tac-Toe</h1>
    </div>
    <div class="game-board">
      <table>
        <tr class="first-row">
          <td class="first-cell" v-on:click="click(0, 0)">
            {{ game[0][0] }}
          </td>
          <td class="second-cell"  v-on:click="click(0, 1)">{{ game[0][1] }}</td>
          <td  v-on:click="click(0, 2)">{{ game[0][2] }}</td>
        </tr>
        <tr class="second-row">
          <td class="first-cell"  v-on:click="click(1, 0)">{{ game[1][0] }}</td>
          <td class="second-cell"  v-on:click="click(1, 1)">{{ game[1][1] }}</td>
          <td  v-on:click="click(1, 2)">{{ game[1][2] }}</td>
        </tr>
        <tr class="third-row">
          <td class="first-cell"  v-on:click="click(2, 0)">{{ game[2][0] }}</td>
          <td class="second-cell"  v-on:click="click(2, 1)">{{ game[2][1] }}</td>
          <td  v-on:click="click(2, 2)">{{ game[2][2] }}</td>
        </tr>
      </table>
       <div v-if="winner">
        <p>
          Game is over!
          <br />
          {{ winner }}
        </p>
       </div>
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
      if(this.winner || this.game[x][y]){
        return;
      }
      this.game[x][y] = 'X';

      // checking all fields is full
      let emptyCount = 0;
      for (let i = 0; i < 3; i++){
        for (let j = 0; j < 3; j++){
          if (!this.game[i][j]){
            emptyCount++;
          }
        }
      }

      if (this.isDone()) {
        this.winner = 'X Winner!';
      } else if (emptyCount === 0){
        this.winner = 'XO Draw!';
      } else{
        this.computerMove();
        if (this.isDone()) {
          this.winner = 'O Winner!';
        }
      }
      this.$forceUpdate();
    }
  },
  data() {
    return {
      winner: '',
      game: [
        [],
        [],
        [],
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
.game-board table .first-row td {
  border-bottom: 1px solid #ccc;
}
.game-board table .second-row td{
  border-bottom: 1px solid #ccc;
}
.game-board table tr .first-cell,
.game-board table tr .second-cell {
  border-right: 1px solid #ccc;
}
.game-board td {
  font-size: 30px;
  width: 60px;
  height: 60px;
  line-height: 60px;
  text-align: center;
}
</style>
