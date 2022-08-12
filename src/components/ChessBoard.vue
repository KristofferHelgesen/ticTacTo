<template>
  <div class="board">
    <div v-for="piece in pieces" :key="piece.id" @click="selectPiece(piece)"
         class="piece"><span
        v-if="piece.symbol !== null">{{ piece.symbol ? "X" : "O" }}</span></div>
  </div>
  <div v-if="whoWon || whoWon === false">
    <p>Congratulation {{ whoWon ? "X" : "O" }} you are the winner!</p>
  </div>
  <div v-if="numberOfClickedItems === 9 && whoWon === null">
    <p>To bad, none of you where able to beat the oponent!</p>
  </div>
</template>

<script>

export default {
  name: "ChessBoard",
  data() {
    return {
      whoIsPlaying: true,
      pieces: [
        {selected: false, id: 0, symbol: null},
        {selected: false, id: 1, symbol: null},
        {selected: false, id: 2, symbol: null},
        {selected: false, id: 3, symbol: null},
        {selected: false, id: 4, symbol: null},
        {selected: false, id: 5, symbol: null},
        {selected: false, id: 6, symbol: null},
        {selected: false, id: 7, symbol: null},
        {selected: false, id: 8, symbol: null}
      ],
      whoWon: null,
      numberOfClickedItems: 0
    }
  },
  methods: {
    selectPiece(selectedPiece) {
      if (this.whoWon !== null) return;
      this.numberOfClickedItems++;
      this.pieces.map((piece) => {
        if (piece.id === selectedPiece.id && piece.selected !== true) {
          piece.selected = true;
          piece.symbol = this.whoIsPlaying;
          this.whoIsPlaying = !this.whoIsPlaying;
        }
        return piece;
      });
      this.hasThreeInARow();
    },
    hasThreeInARow() {

      const possibilities = [
        [0, 4, 8],
        [2, 4, 6],
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
      ];

      for (let i = 0; i < possibilities.length; i++) {

        const possibility = possibilities[i];
        const a = this.pieces[possibility[0]];
        const b = this.pieces[possibility[1]];
        const c = this.pieces[possibility[2]];

        if (a.symbol && b.symbol && c.symbol) {
          this.whoWon = true;
        } else if (a.symbol === false && b.symbol === false && c.symbol === false) {
          this.whoWon = false;
        }
      }
    }
  },
}
</script>

<style scoped>

.board {
  height: 500px;
  width: 500px;
  margin: auto;
  border: 5px solid black;
  display: flex;
  justify-content: space-evenly;
  flex-flow: wrap;
  padding: 10px;
}

.piece {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  width: 150px;
  height: 150px;
  text-align: center;
  border: 1px solid black;
  font-size: 20px;
}


</style>
