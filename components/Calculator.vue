<template>
  <form class="grid grid-cols-2 gap-4">
    <div>
      <div class="flex flex-col mb-4">
        <label for="x1">Width 1</label>
        <input
          class="border"
          v-model="x1"
          v-on:input="solve(undefined, y2, x1, y1)"
          id="x1"
          type="text"
        />
      </div>

      <div class="flex flex-col">
        <label for="y1">Height 1</label>
        <input
          class="border"
          v-model="y1"
          v-on:input="solve(x2, undefined, x1, y1)"
          id="y1"
          type="text"
        />
      </div>
    </div>

    <div>
      <div class="flex flex-col mb-4">
        <label for="x2">Width 2</label>
        <input
          class="border"
          v-model="x2"
          v-on:input="solve(x2, undefined, x1, y1)"
          id="x2"
          type="text"
        />
      </div>

      <div class="flex flex-col">
        <label for="y2">Height 2</label>
        <input
          class="border"
          v-model="y2"
          v-on:input="solve(undefined, y2, x1, y1)"
          id="y2"
          type="text"
        />
      </div>
    </div>

    <pre>
      x1: {{x1}}
      y1: {{y1}}
      x2: {{x2}}
      y2: {{y2}}
      ratio: {{ratio}}
    </pre>
  </form>
</template>

<script>
export default {
  data: function() {
    return {
      x1: 1920,
      y1: 1080,
      x2: 0,
      y2: 0,
      ratio: "16:9"
    };
  },
  methods: {
    /**
     * Solve for the 4th value
     * @param Number width       Numerator from the right side of the equation
     * @param Number height      Denominator from the right side of the equation
     * @param Number numerator   Numerator from the left side of the equation
     * @param Number denominator Denominator from the left side of the equation
     * @return Number
     */
    solve: function(width, height, numerator, denominator) {
      // solve for width
      if (undefined !== width) {
        this.y2 = Math.round(width / (numerator / denominator));
      }
      // solve for height
      else if (undefined !== height) {
        this.x2 = Math.round(height * (numerator / denominator));
      } else {
        console.error("failed");
      }

      this.reduceRatio(this.x1, this.y1);
    },
    reduceRatio: function(numerator, denominator) {
      var gcd, temp, divisor;

      gcd = function(a, b) {
        if (b === 0) return a;
        return gcd(b, a % b);
      };

      if (numerator === denominator) return "1 : 1";

      if (+numerator < +denominator) {
        temp = numerator;
        numerator = denominator;
        denominator = temp;
      }

      divisor = gcd(+numerator, +denominator);

      this.ratio =
        "undefined" === typeof temp
          ? numerator / divisor + " : " + denominator / divisor
          : denominator / divisor + " : " + numerator / divisor;
    }
  }
};
</script>

<style>
</style>
