<template>
  <div class="center">
    <vs-dialog width="300px" not-center v-model="active">
      <h4 class="not-margin">
        Welcome to <b>Tic-Tac-Toe</b>, please type your names.
      </h4>

      <div class="con-content">
        <vs-input v-model="input1" placeholder=" Player one name">
          <template #icon>
            <i class="bx bx-user"></i>
          </template>
        </vs-input>
      </div>
      <div class="con-content">
        <vs-input v-model="input2" placeholder=" Player two name">
          <template #icon>
            <i class="bx bx-user"></i>
          </template>
        </vs-input>
      </div>

      <div class="rounds">
        <h4 class="not-margin h4-center">Select rounds:</h4>
        <vs-radio danger v-model="picked" class="radio-s" :val="one">
          Just one game
        </vs-radio>
        <vs-radio danger v-model="picked" class="radio-s" :val="three">
          Best of 3
        </vs-radio>
        <vs-radio danger v-model="picked" class="radio-s" :val="five">
          Best of 5
        </vs-radio>
      </div>

      <template #footer>
        <div class="con-footer">
          <vs-button @click="submitData" transparent> Ok </vs-button>
          <vs-button @click="active = false" dark transparent>
            Cancel
          </vs-button>
        </div>
      </template>
    </vs-dialog>
  </div>
</template>

<script>
export default {
  name: "PlayersName",
  data() {
    return {
      active: false,
      input1: "",
      input2: "",
      picked: 1,
      one: 1,
      three: 3,
      five: 5,
    };
  },
  methods: {
    promptDeley() {
      setTimeout(() => {
        this.active = true;
      }, 1000);
    },
    submitData() {
      this.$emit("sendPayload", [this.input1, this.input2, this.picked]);
      this.active = false;
    },
  },
  created() {
    this.promptDeley();
  },
};
</script>

<style>
.con-footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
.con-footer .vs-button {
  margin: 0px;
}
.con-footer .vs-button .vs-button__content {
  padding: 10px 30px;
}
.con-footer .vs-button ~ .vs-button {
  margin-left: 10px;
}
.not-margin {
  margin: 0px auto;
  font-weight: normal;
  padding: 10px;
  padding-bottom: 0px;
}
.con-content {
  width: 100%;
}
.con-content p {
  font-size: 0.8rem;
  padding: 0px 10px;
}
.con-content .vs-checkbox-label {
  font-size: 0.8rem;
}
.con-content .vs-input-parent {
  width: 100%;
}
.con-content .vs-input-content {
  margin: 10px 0px;
  width: calc(100%);
}
.con-content .vs-input-content .vs-input {
  width: 100%;
}
.footer-dialog {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: calc(100%);
}
.footer-dialog .new {
  margin: 0px;
  margin-top: 20px;
  padding: 0px;
  font-size: 0.7rem;
}
.footer-dialog .new a {
  color: rgba(var(--vs-primary), 1) !important;
  margin-left: 6px;
}
.footer-dialog .new a:hover {
  text-decoration: underline;
}
.footer-dialog .vs-button {
  margin: 0px;
}

.con-content:first-of-type {
  margin-top: 10px;
}

.h4-center {
  margin-bottom: 20px;
}
.radio-s {
  display: flex !important;
  justify-content: left !important;
  padding-left: 20px !important;
  margin-top: 10px !important;
}
</style>
