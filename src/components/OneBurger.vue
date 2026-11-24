<template>
  <div style="display: inline-block">
                <h3>{{ burger.name }}</h3>
                <img :src="burger.url", style="width: 200px">
                <ul>
                    <li>{{ burger.kCal }} kCal</li>
                    <li v-if="burger.lactose">Contains <span class="ingredients">lactose</span></li>
                    <li v-if="burger.gluten">Contains <span class="ingredients">gluten</span></li>
                </ul>
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
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ingredients {
    color: #ff5500;
    font-weight: bold;
}

#burgers {
    background-color: black;
    color: white;
    padding: 10px;
    border: 4px dotted white;
}

#burgers p {
    color: white;
}
#burgers div {
    padding: 10px;
    margin: 10px;
    text-align: left;
}
.burger-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-column-gap: 20px;
}
</style>