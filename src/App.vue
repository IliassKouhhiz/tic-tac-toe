<template>
  <div>
    <players-name @sendPayload="uploadInfo"></players-name>
    <result-alert
      :name_one_prop="name_one"
      :name_two_prop="name_two"
      :status="status"
      :current_round_prop="current_round"
      :rounds_prop="rounds"
      @reset="inputContinue"
    ></result-alert>
    <header>
      <h1>.Tic Tac Toe</h1>
      <score-counter
        :name_one_prop="name_one"
        :name_two_prop="name_two"
        :rounds_prop="rounds"
        :pointsOne="points_one"
        :pointsTwo="points_two"
        :player_turn="moves_count"
      ></score-counter>
      <div v-if="playButton" class="playButton">
        <vs-button success @click="refresh"> Play again </vs-button>
      </div>
    </header>
    <div class="grid">
      <grid-component
        v-for="slot in slots"
        :key="slot.index"
        :id="slot.name"
        :identifier="slot.name"
        @clicked="selectSign"
        :button_key="buttonKey"
        :isValid="gridClick"
      >
        {{ displaySign(slot.name) }}
      </grid-component>
    </div>
  </div>
</template>

<script>
import GridComponent from "./components/GridComponent.vue";
import ScoreCounter from "./components/ScoreCounter.vue";
import PlayersName from "./components/PlayersName.vue";
import ResultAlert from "./components/ResultAlert.vue";

export default {
  name: "App",
  components: {
    GridComponent,
    ScoreCounter,
    PlayersName,
    ResultAlert,
  },
  data() {
    return {
      name_one: "",
      name_two: "",
      rounds: 1,
      current_round: 0,
      points_one: 0,
      points_two: 0,
      slots: [],
      moves_count: [],
      id_sign: [],
      sign: null,
      clicked_id: null,
      status: "default",
      buttonKey: 1,
      gridClick: true,
      playButton: false,
    };
  },
  methods: {
    uploadInfo(data) {
      this.name_one = data[0];
      this.name_two = data[1];
      this.rounds = data[2];
    },
    gridLoop() {
      for (let i = 0; i <= 8; i++) {
        this.slots.push({ name: i });
      }
    },
    selectSign(value) {
      this.moves_count.push(value);
      this.moves_count.length % 2 !== 0
        ? this.id_sign.push({ id: value, sign: "x" })
        : this.id_sign.push({ id: value, sign: "O" });
      this.clicked_id = value;
    },
    displaySign(value) {
      let result = this.id_sign.find((item) => item.id === value);
      return result === undefined ? "" : result.sign;
    },
    gameLogic(first, second, third) {
      if (
        this.moves_count.includes(first) &&
        this.moves_count.includes(second) &&
        this.moves_count.includes(third)
      ) {
        let combo = this.id_sign.filter(
          (item) => item.id === first || item.id === second || item.id === third
        );
        if (combo === undefined) {
          console.log("no");
        } else {
          let arr = [];
          for (const key in combo) {
            arr.push(combo[key].sign);
          }
          this.win(arr);
        }
      }
    },
    win(arr) {
      if (this.rounds === 1) {
        if (arr.every((value) => value === "x")) {
          this.status = "x wins";
          this.points_one++;
          this.playAgain();
        } else if (arr.every((value) => value === "O")) {
          this.status = "O wins";
          this.points_two++;
          this.playAgain();
        }
      }

      if (this.rounds === 3) {
        if (this.current_round === 3 && arr.every((value) => value === "x")) {
          this.updateRounds();
          this.points_one++;
        } else if (
          this.current_round === 3 &&
          arr.every((value) => value === "O")
        ) {
          this.updateRounds();
          this.points_two++;
        } else if (
          this.current_round < 3 &&
          arr.every((value) => value === "x")
        ) {
          this.updateRounds();
          this.status = "x wins round";
          this.points_one++;
          this.resetGame();
        } else if (
          this.current_round < 3 &&
          arr.every((value) => value === "O")
        ) {
          this.updateRounds();
          this.status = "O wins round";
          this.points_two++;
          this.resetGame();
        }
      }

      if (this.rounds === 5) {
        if (this.current_round === 5 && arr.every((value) => value === "x")) {
          this.updateRounds();
          this.points_one++;
        } else if (
          this.current_round === 5 &&
          arr.every((value) => value === "O")
        ) {
          this.updateRounds();
          this.points_two++;
        } else if (
          this.current_round < 5 &&
          arr.every((value) => value === "x")
        ) {
          this.updateRounds();
          this.status = "x wins round";
          this.points_one++;
          this.resetGame();
        } else if (
          this.current_round < 5 &&
          arr.every((value) => value === "O")
        ) {
          this.updateRounds();
          this.status = "O wins round";
          this.points_two++;
          this.resetGame();
        }
      }
    },
    draw() {
      if (this.rounds === 1) {
        if (this.status === "default" && this.moves_count.length == 9) {
          this.status = "draw";
          this.updateRounds();
          this.playAgain();
        }
      }

      if (this.rounds === 3) {
        if (
          this.status === "default" &&
          this.moves_count.length == 9 &&
          this.current_round == 3
        ) {
          this.updateRounds();
          this.playAgain();
        }
        if (
          this.status === "default" &&
          this.moves_count.length == 9 &&
          this.current_round < 3
        ) {
          this.status = "draw round";
          this.updateRounds();
          this.resetGame();
        }
      }

      if (this.rounds === 5) {
        if (
          this.status === "default" &&
          this.moves_count.length == 9 &&
          this.current_round == 5
        ) {
          this.updateRounds();
          this.playAgain();
        }
        if (
          this.status === "default" &&
          this.moves_count.length == 9 &&
          this.current_round < 5
        ) {
          this.status = "draw round";
          this.updateRounds();
          this.resetGame();
        }
      }
    },
    gameover() {
      if (
        this.rounds === this.current_round &&
        this.points_one > this.points_two
      ) {
        this.status = "x wins";
        this.playAgain();
      }
      if (
        this.rounds === this.current_round &&
        this.points_one < this.points_two
      ) {
        this.status = "O wins";
        this.playAgain();
      }
      if (
        this.rounds === this.current_round &&
        this.points_one === this.points_two
      ) {
        this.status = "draw";
        this.playAgain();
      }
    },
    updateRounds() {
      if (this.rounds > 1) {
        ++this.current_round;
      }
    },
    playAgain() {
      this.gridClick = false;
      this.moves_count.length = 0;
      this.id_sign.length = 0;
      this.sign = null;
      this.clicked_id = null;
      this.playButton = true;
    },
    resetGame() {
      setTimeout(() => {
        this.moves_count.length = 0;
        this.id_sign.length = 0;
        this.sign = null;
        this.clicked_id = null;
        this.buttonKey++;
      }, 1000);
    },
    inputContinue(value) {
      if (value === true) {
        this.status = "default";
      }
    },
    refresh() {
      window.location.reload();
    },
  },
  watch: {
    moves_count: function () {
      this.gameLogic(0, 1, 2);
      this.gameLogic(3, 4, 5);
      this.gameLogic(6, 7, 8);
      this.gameLogic(0, 3, 6);
      this.gameLogic(1, 4, 7);
      this.gameLogic(2, 5, 8);
      this.gameLogic(0, 4, 8);
      this.gameLogic(2, 4, 6);
      this.draw();
      if (this.moves_count.length === 1) {
        this.status = "default";
      }
    },
    current_round: function () {
      this.gameover();
    },
  },
  created() {
    this.gridLoop();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

body {
  height: 100vh;
  background-color: #eeeeea;
  overflow: hidden;
}

header {
  width: 90%;
  display: flex;
  justify-content: space-between;
  margin: 30px auto;
}
header > h1 {
  color: #2e3031;
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  grid-column-gap: 10px;
  grid-row-gap: 10px;
  background-color: #5d6063;
  width: 460px;
  height: 460px;
  margin: -80px auto;
  border-radius: 30px;
}

.playButton {
  position: absolute;
  top: 140px;
  height: 40px;
}
</style>
