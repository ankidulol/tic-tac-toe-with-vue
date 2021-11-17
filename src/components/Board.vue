<template>
  <div class="board">
    <div
      class="square"
      v-for="square in squares"
      :key="square.id"
      @click="clickedSquare(square)"
    >
      <p>{{ square.pressed }}</p>
    </div>
  </div>
  <div class="score">
    <div>
      <h1>X</h1>
      <h2>{{ x }}</h2>
    </div>
    <div>
      <h1>O</h1>
      <h2>{{ o }}</h2>
    </div>
  </div>
  <button class="play-again" @click="playAgain">Play Again</button>
</template>

<script>
export default {
  name: "Board",
  data() {
    let squares = [];
    for (let i = 1; i <= 9; i++) {
      squares = [...squares, { id: i, pressed: undefined }];
    }

    return {
      squares,
      round: 1,
      whoWon: "",
      x: 0,
      o: 0,
    };
  },
  methods: {
    givePoint(to) {
      if (to == "x") this.x++;
      else this.o++;
    },
    checkRows() {
      const horizontal = [
        this.squares.slice(0, 3),
        this.squares.slice(3, 6),
        this.squares.slice(-3),
      ];
      for (const horizontalRow of horizontal) {
        let winning = [];
        for (const row of horizontalRow) {
          if (!row.pressed) break;

          if (!winning.length) {
            winning = [...winning, row.pressed];
          } else {
            if (winning[winning.length - 1] == row.pressed) {
              winning = [...winning, row.pressed];
              if (winning.length == 3) return (this.whoWon = winning[0]);
            }
          }
        }
      }
    },
    checkColumns() {
      for (let i = 0; i < 3; i++) {
        let column = [];
        for (let j = i; j < 9; j += 3) {
          const { pressed } = this.squares[j];
          if (!pressed) break;

          if (!column.length) {
            column = [...column, pressed];
          } else {
            if (column[column.length - 1] == pressed) {
              column = [...column, pressed];
              if (column.length == 3) return (this.whoWon = column[0]);
            }
          }
        }
      }
    },
    checkAngled() {
      for (let i = 0; i <= 2; i += 2) {
        let angled = [];
        for (let j = i; j < 9; j += 4 - i) {
          const { pressed } = this.squares[j];
          if (!pressed) break;

          if (!angled.length) {
            angled = [...angled, pressed];
          } else {
            if (angled[angled.length - 1] == pressed) {
              angled = [...angled, pressed];
              if (angled.length == 3) return (this.whoWon = angled[0]);
            }
          }
        }
      }
    },
    checkWinner() {
      if (this.checkRows() || this.checkColumns() || this.checkAngled()) {
        console.log("WINNER IS ", this.whoWon);
        this.givePoint(this.whoWon);
        return;
      }
    },
    clickedSquare(square) {
      if (square.pressed || this.whoWon) return;

      this.squares.splice(square.id - 1, 1, {
        ...square,
        pressed: this.round % 2 == 0 ? "o" : "x",
      });

      if (this.round >= 5) this.checkWinner();

      this.round++;
    },
    playAgain() {
      this.whoWon = "";
      this.squares = this.squares.map(sq => {
        if (sq.pressed) return { ...sq, pressed: undefined };
        return sq;
      });
    },
  },
};
</script>

<style scoped>
.board {
  display: flex;
  flex-wrap: wrap;
  margin: auto;
  width: 30em;
  box-shadow: 0px 2px 9px #b1b1b1;
}
.square {
  display: flex;
  width: 10em;
  height: 10em;
  background-color: #c7c7d2;
}
.square:nth-child(odd) {
  background-color: #e1e1ea;
}
h2 {
  padding-left: 0.5em;
  color: #c7c7d2;
}
p {
  color: #525d67;
  font-family: Permanent Marker;
  margin: 0;
  font-size: 6em;
  display: flex;
  flex: 1;
  align-items: center;
  justify-content: center;
}
.score {
  display: flex;
  width: 30em;
  margin: auto;
  justify-content: space-evenly;
}
.score > div {
  display: flex;
  align-items: center;
}
.play-again {
  border: none;
  background: #7b9fd8;
  border-radius: 10px;
  padding: 1em;
  box-shadow: 1px 1px 5px #b7b7b7;
  font-size: 1.1em;
  font-weight: bold;
  color: #fff;
  cursor: pointer;
}
</style>
