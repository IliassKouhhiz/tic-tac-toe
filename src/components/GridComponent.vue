<template>
  <div class="cnt">
    <div
      class="grid-component"
      :class="{ clicked: isClicked, not_clickable: !isValid }"
      @click.once="clicked"
      :key="button_key"
    >
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "GridComponent",
  props: {
    identifier: Number,
    button_key: Number,
    isValid: Boolean,
  },
  data() {
    return {
      id: this.identifier,
      isClicked: false,
      click: null,
      sign: null,
      clicked_slots: [],
      valid: true,
    };
  },
  methods: {
    clicked() {
      if (this.isValid) {
        this.click = this.id;
        this.$emit("clicked", this.click);
        this.isClicked = true;
      }
    },
    removeClickEvent() {
      if (!this.isValid) {
        this.isClicked = true;
      }
    },
  },
  watch: {
    button_key: function () {
      this.isClicked = false;
    },
    isValid: function () {
      this.removeClickEvent();
    },
  },
};
</script>

<style scoped>
.cnt {
  padding: 10px;
  background-color: #eeeeea;
}

.grid-component {
  height: 100%;
  border-radius: 20px;
  background-color: #eeeeea;
  cursor: pointer;
  font-size: 80px;
  text-align: center;
  color: #2e3031;
}

.grid-component:hover {
  transition: all 0.2s ease-in-out;
  background-color: #e0e0de;
}

.clicked:hover,
.not_clickable {
  background-color: #eeeeea;
  cursor: default;
}
</style>
