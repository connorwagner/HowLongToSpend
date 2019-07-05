<template>
  <div class="calculator">
    <label for="pay-amount">$</label>
    <input v-model="payAmount" type="number" min="0" name="pay-amount" />
    <div class="hourly-salary-picker">
      <label for="hourly-radio">Hourly</label>
      <input v-model="hourlyOrSalary" type="radio" value="hourly" />
      <label for="salary-radio">Salary</label>
      <input v-model="hourlyOrSalary" type="radio" value="salary" />
    </div>
    <label for="material-cost">Cost for materials</label>
    <input v-model="materialCost" type="number" min="0" name="material-cost" />
    <label for="purchase-cost">Cost to purchase</label>
    <input v-model="purchaseCost" type="number" min="0" name="purchase-cost" />
    <p v-show="isValidTimeString">
      You can spend {{ breakEvenTimeString }} hours before it is worth making
      the purchase.
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      hourlyOrSalary: "hourly",
      payAmount: 0,
      materialCost: 0,
      purchaseCost: 0
    };
  },
  computed: {
    hourlyPay() {
      if (this.hourlyOrSalary === "hourly") {
        return this.payAmount;
      } else {
        return this.payAmount / 40 / 52;
      }
    },
    hoursToBreakEven() {
      return (this.purchaseCost - this.materialCost) / this.hourlyPay;
    },
    breakEvenTimeString() {
      const padWith0 = num => {
        if (num < 10) {
          return "0" + num;
        } else {
          return num;
        }
      };

      const hourString = padWith0(Math.floor(this.hoursToBreakEven));
      const minuteString = padWith0(
        Math.floor(this.hoursToBreakEven * 60) % 60
      );
      return hourString + ":" + minuteString;
    },
    isValidTimeString() {
      return this.breakEvenTimeString.length === 5;
    }
  }
};
</script>

<style lang="scss" scoped>
div.calculator {
  border: 3px solid black;
  width: 19em;

  display: grid;
  grid-template:
    "hourly-salary-picker pay-amount-label    pay-amount-input   " 2em
    "material-cost-label  material-cost-label material-cost-input" 2em
    "purchase-cost-label  purchase-cost-label purchase-cost-input" 2em
    "output               output              output             " 1fr
    / 8em 1em 10em;

  > label {
    margin: 0;
    margin-right: 1em;

    display: flex;
    flex-flow: row nowrap;
    justify-content: flex-end;
    align-items: center;

    &[for="pay-amount"] {
      grid-area: pay-amount-label;
    }

    &[for="material-cost"] {
      grid-area: material-cost-label;
    }

    &[for="purchase-cost"] {
      grid-area: purchase-cost-label;
    }
  }

  > input {
    &[name="pay-amount"] {
      grid-area: pay-amount-input;
    }

    &[name="material-cost"] {
      grid-area: material-cost-input;
    }

    &[name="purchase-cost"] {
      grid-area: purchase-cost-input;
    }
  }

  > div.hourly-salary-picker {
    grid-area: hourly-salary-picker;

    display: grid;
    grid-template:
      "hourly-radio-input hourly-radio-label" 1em
      "salary-radio-input salary-radio-label" 1em
      / 2em 1fr;

    > label {
      &[for="hourly-radio"] {
        grid-area: hourly-radio-label;
      }

      &[for="salary-radio"] {
        grid-area: salary-radio-label;
      }
    }

    > input {
      &[name="hourly-radio"] {
        grid-area: hourly-radio-input;
      }

      &[name="salary-radio"] {
        grid-area: salary-radio-input;
      }
    }
  }

  > p {
    margin: 0;
    margin-left: 1em;
    margin-right: 1em;
    grid-area: output;
  }
}
</style>
