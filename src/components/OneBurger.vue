<template>
  <div class="burgerItem">
        <h3>{{ burger.name }}</h3>
        <img :src="burger.url", style="width: 200px">
        <ul>
            <li>{{ burger.kCal }} kCal</li>
            <li v-if="burger.lactose">Contains <span class="ingredients">lactose</span></li>
            <li v-if="burger.gluten">Contains <span class="ingredients">gluten</span></li>
        </ul>
        <div class="counter">
            <button class="counterButton" v-on:click="subAmount">
                -
            </button>
            <span class="amount">{{ amountOrdered }}</span>
            <button class="counterButton" v-on:click="addAmount">
                +
            </button>
        </div>
    </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function() {
    return {
        amountOrdered: 0,
    }
  },
  methods: {
    addAmount: function () {
        this.amountOrdered += 1;
        this.$emit("orderedBurgers", {name: this.burger.name, amount: this.amountOrdered}
            );
        },
    subAmount: function () {
        if (this.amountOrdered > 0) {
            this.amountOrdered += -1;
            this.$emit("orderedBurgers", {name: this.burger.name, amount: this.amountOrdered}
            );
        }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.burgerItem {
    display: flex;
    flex-direction: column;
    height: 100%;
}

.burgerItem img {
    margin: 0 auto;
}

.counter {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
    margin-top: auto;
}
.counterButton {
    
    margin: 0;
}
.amount {
    min-width: 25px;
    text-align: center;
}
</style>