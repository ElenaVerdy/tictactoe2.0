<template>
  <div id="app">
    <div>{{ score }}</div>
    <div class="bottom">
      <div class="main">
        <div v-for="(item, i) in field" :key="i" class="cell" @click="onClick(i)">
			{{ item }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data () {
		return {
			field: ['', '', '', '', '', '', '', '', ''],
			playerWon: 0,
			pcWon: 0,
			pcTurn: false
		};
    },
    computed: {
		score () {
			return `${this.playerWon}:${this.pcWon}`;
		},
		gameOver() {
			return this.field.indexOf('') === -1;
		}
    },
    methods: {
		onClick (i) {
			if (this.field[i] || this.pcTurn) return;
			this.makeMove(i);
		},
		finishMove() {
			if (this.checkForTheWin()) return this.gameWon();
			if (this.gameOver) return this.restart();
			this.nextMove();
		},
		gameWon() {
			if (this.pcTurn) this.playerWon++;
			else this.pcWon++;
			this.restart();
		},
		restart() {
			this.field = ['', '', '', '', '', '', '', '', ''];
			this.pcTurn = false;
		},
		makeMove(index) {
			this.field[index] = this.pcTurn ? "O" : "X";
			this.field = this.field.slice();
			this.finishMove()
		},
		nextMove() {
			this.pcTurn = !this.pcTurn;
			if (this.pcTurn) setTimeout(() => {
				this.makeComputerMove();
			}, Math.random() * 500);
		},
		makeComputerMove () {
			this.makeMove(this.field.indexOf(""));
		},
		checkForTheWin() {
			let ret = [0, 1, 2].some(i => {
				if (this.field[i] && this.field[i] === this.field[i + 3] && this.field[i] === this.field[i + 6]) return true;
			});
			
			ret = ret || [0, 3, 6].some(i => {
				if (this.field[i] && this.field[i] === this.field[i + 1] && this.field[i] === this.field[i + 2]) return true;
			});

			ret = ret || (this.field[0] && this.field[0] === this.field[4] && this.field[0] === this.field[8]);
			ret = ret || (this.field[2] && this.field[2] === this.field[4] && this.field[2] === this.field[6]);

			return ret;
		}
	}
  }
</script>

<style>
.main {
  display: flex;
  width: 90px;
  flex-wrap: wrap;
}
.cell {
  width: 30px;
  height: 30px;
  box-sizing: border-box;
  border: 1px solid black;
}
</style>
