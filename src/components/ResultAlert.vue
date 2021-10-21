<template>
  <div>
    <div v-if="status === 'x wins' || status === 'O wins'" class="center" id="final-win">
      <vs-dialog width="550px" not-center not-close v-model="active">
        <h4 class="not-margin">
          Congratualtion <b>{{result("winner")}}</b>, you won!
        </h4>
        <img src="@/assets/img/winner.gif" width=450 height=330.000>
        <div>
          <h4 class="not-margin">
            Hey <b>{{result("loser")}}</b>! You suck at this!
          </h4>
        </div>
        <template #footer>
          <div class="con-footer">
            <vs-button @click="refresh" success>
              Play Again
            </vs-button>
          </div>
        </template>
      </vs-dialog>
    </div>
    
    <div v-if="status === 'x wins round' || status === 'O wins round'" class="center" id="round-win">
      <vs-dialog width="550px" not-center not-close v-model="active">
        <h4 class="not-margin">
          <b>{{result("winner")}}</b> wins the {{returnText(current_round_prop)}} round.
        </h4>
        <div class="con-content">
          <h4  class="not-margin">
            Hey <b>{{result("loser")}}</b>! You need to step up your game!
          </h4>
        </div>
        <template #footer>
          <div class="con-footer">
            <vs-button @click="inputContinue" transparent>
              Continue
            </vs-button>
          </div>
        </template>
      </vs-dialog>
    </div>

    <div v-if="status === 'draw round'" class="center" id="round-draw">
      <vs-dialog width="550px" not-center not-close v-model="active">
        <h4 class="not-margin">
          <b>{{result("winner")}}</b>, <b>{{result("loser")}},</b> we have a tie for the {{returnText(current_round_prop)}} round.
        </h4>
        <div class="con-content">
          <h4  class="not-margin">
            That was a cat's game!
          </h4>
        </div>
        <template #footer>
          <div class="con-footer">
            <vs-button @click="inputContinue" transparent>
              Continue
            </vs-button>
          </div>
        </template>
      </vs-dialog>
    </div>

    <div v-if="status === 'draw'" class="center" id="final-draw">
      <vs-dialog width="550px" not-center not-close v-model="active">
        <h4 class="not-margin">
          <b>{{result("winner")}}</b>, <b>{{result("loser")}}</b> let's just call it a draw.
        </h4>
        <img src="@/assets/img/loser.gif" width=450 height=330.000>
        <div>
          <h4 class="not-margin">
            Say what? A tiebrake? I got you, just hit the green button.
          </h4>
        </div>
        <template #footer>
          <div class="con-footer">
            <vs-button @click="refresh" success>
              Play Again
            </vs-button>
          </div>
        </template>
      </vs-dialog>
    </div>

  </div>
</template>

<script>

export default {
  name: "ResultAlert",
  props: {
    name_one_prop: String,
    name_two_prop: String,
    current_round_prop: Number,
    rounds_prop: Number,
    status: String
  },
  data() {
    return {
      active: false,
      reset: false
    };
  },
  computed: {
    name_one: {
      get: function () {
       return this.name_one_prop === "" ? "Player One" : this.name_one_prop 
      }
    },
    name_two: {
      get: function () {
       return this.name_two_prop === "" ? "Player Two" : this.name_two_prop 
      }
    },
  },
  methods: {
    updateStatus() { if(this.status !== "default") { this.active = true } }, 
    refresh() { window.location.reload() },
    result(id) { 
      if(this.status === "x wins" || this.status === "x wins round") { return id === "winner" ? this.name_one : this.name_two } 
      else { return id === "winner" ? this.name_two : this.name_one }
    },
    returnText(n) {
      if(n == 1) { return "first"}
      if(n == 2) { return "second"}
      if(n == 3) { return "third"}
      if(n == 4) { return "fourth"}
    },
    inputContinue() {
      this.reset = true;
      this.$emit('reset', this.reset);
      setTimeout(() => {
        this.reset = false;
      }, 1000)
    }
  },
  watch: {
    status: function() {
      this.updateStatus();
    }
  }
}
</script>

<style>
  img {
    margin: 10px 30px 0px;
  }
</style>