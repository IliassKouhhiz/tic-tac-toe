<template>
  <div class="score">    
    <vs-card type="3">
      <template #title>
        <h3>Score:</h3>
      </template>
      <template #text>
        <div class="players-cnt">
          <div class="player" :class="{focused_one : player}">
            <p>{{ nameOne || "Player One" }}</p>
            <p>{{ pointsOne }} / {{rounds_prop}}</p>
          </div>
          <div class="player" :class="{focused_two : !player}">
            <p>{{ nameTwo || "Player Two" }}</p>
            <p>{{ pointsTwo }} / {{rounds_prop}}</p>
          </div>
        </div>
      </template>
    </vs-card>
    </div>
</template>

<script>

export default {
  name: "ScoreCounter",
  props: {
    name_one_prop: String,
    name_two_prop: String,
    rounds_prop: Number,
    pointsOne: Number,
    pointsTwo: Number,
    player_turn: Array   
  },
  data() {
    return {
      player: true
    };
  },
  computed: {
    nameOne: {
      get() {
        return this.name_one_prop !== "" ? this.name_one_prop : null;
      },
    },
    nameTwo: {
      get() {
        return this.name_two_prop !== "" ? this.name_two_prop : null;
      },
    },
  },
  methods: {
    playerTurn() {
      this.player_turn.length % 2 == 0 ? this.player = true : this.player = false;
    }
  },
  watch: {
    player_turn: function() {
      this.playerTurn();
      console.log(this.player_turn);
    }
  }
};
</script>

<style>
  .score {
    width: 250px;
  }
  .vs-card__text {
    width: 100%;
  }
  .vs-card__title > h3 {
    text-align: center;
  }

  .players-cnt {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
    margin: 10px auto;
  }
  .player {
    width: 45%;
    border-radius: 10px;
    padding: 10px;
  }
  .player > p {
    text-align: center;
    color: #eeeeee;
  } 
  .player:first-of-type {
    background-color: #181bd9;
  }
  .player:last-of-type {
    background-color: #d91818;
  }

  .focused_one {
    box-shadow: -1px 2px 18px 5px rgba(24,27,217,0.66);
  }
  .focused_two {
    box-shadow: -1px 2px 18px 5px rgba(217,24,24,0.66);
  }
</style>