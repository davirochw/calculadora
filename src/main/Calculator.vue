<template>
  <div>
    <div class="container mt-4 d-flex justify-content-center text-light">
      <div class="calculator col-6 shadow p-3 mb-5 rounded">
        <div id="resultado" class="text-light text-right justify-content-end">
          {{ displayValue }}
        </div>
        <div class="row">
          <Button
            label="C"
            class="col-6 col-sm-6 btn-lg btn-danger"
            @onClick="clearMemory"
          />
          <Button
            label="/"
            class="col-6 col-sm-6 btn-lg btn-secondary"
            @onClick="setOperation"
          />
        </div>
        <div class="row">
          <Button
            label="7"
            class="col-6 col-sm-3 btn-lg btn-secondary "
            @onClick="addDigit"
          />
          <Button
            label="8"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="9"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="*"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="setOperation"
          />
        </div>
        <div class="row">
          <Button
            label="4"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="5"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="6"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="-"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="setOperation"
          />
        </div>
        <div class="row">
          <Button
            label="1"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="2"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="3"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="+"
            class="col-6 col-sm-3 btn-lg btn-secondary"
            @onClick="setOperation"
          />
        </div>
        <div class="row">
          <Button
            label="0"
            class="col-6 col-sm-4 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="."
            class="col-6 col-sm-4 btn-lg btn-secondary"
            @onClick="addDigit"
          />
          <Button
            label="="
            class="col-6 col-sm-4 btn-lg btn-warning"
            @onClick="setOperation"
          />
        </div>
      </div>
    </div>
    <Me />
  </div>
</template>

<script>
import Button from "../components/Button.vue";
import Me from "../components/Me.vue";

export default {
  components: { Me, Button },
  data() {
    return {
      displayValue: "0",
      clearDisplay: false,
      operation: null,
      values: [0, 0],
      current: 0,
    };
  },
  methods: {
    clearMemory() {
      Object.assign(this.$data, this.$options.data());
    },
    setOperation(operation) {
      if (this.current === 0) {
        this.operation = operation;
        this.current = 1;
        this.clearDisplay = true;
      } else {
        const equals = operation === "=";
        const currentOperation = this.operation;

        const values = [...this.values];
        try {
          this.values[0] = eval(
            `${values[0]} ${currentOperation} ${values[1]}`
          );
          if (isNaN(this.values[0]) || !isFinite(this.values[0])) {
            this.ClearMemory();
            return;
          }
        } catch (e) {
          this.$emit("onError", e);
        }

        this.values[1] = 0;

        this.displayValue = this.values[0];
        this.operation = equals ? null : operation;
        this.current = equals ? 0 : 1;
        this.clearDisplay = !equals;
      }
    },
    addDigit(n) {
      if (n === "." && this.displayValue.includes(".")) return;

      const clearDisplay = this.displayValue === "0" || this.clearDisplay;
      const currentValue = clearDisplay ? "" : this.displayValue;
      const displayValue = currentValue + n;

      this.displayValue = displayValue;
      this.clearDisplay = false;
      this.values[this.current] = displayValue;
    },
  },
};
</script>

<style>
#resultado {
  text-align: right;
  display: block;
  width: 100%;
}
</style>
