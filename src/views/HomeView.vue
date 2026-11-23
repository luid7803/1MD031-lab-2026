<template>
<header>
        <img src="/img/funny-header.jpg">
        <h1>Welcome to BurgerShopOnline</h1>
    </header>
    <main>
        <section id="burgers">
            <h2>Select burger</h2>
            <p>This is where you select your burger<br></p>
        <div class="burger-grid">
            <Burger
              v-for="burger in burgers"
              :key="burger.name"
              :burger="burger"
            />
        </div>
        </section>

        <section id="contact">
            <h2>Delivery information</h2>
            <form>
                <p>
                    <label for="firstname">Full name</label><br>
                    <input type="text" id="firstname" name="fn" required="required" placeholder="First- and Last name">
                </p>
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="email" id="email" name="em" required="required" placeholder="E-mail address">
                </p>
                <p>
                    <label for="street">Street</label><br>
                    <input type="text" id="street" name="st" placeholder="Street name">
                </p>
                <p>
                    <label for="house">House</label><br>
                    <input type="number" id="house" name="hs" placeholder="House number">
                </p>
                <h3>Please select your gender</h3>
                <p>
                    <label  for="male">Male</label><br>
                    <input type="radio" id="male" name="gender">
                </p>
                <p>
                    <label  for="female">Female</label><br>
                    <input type="radio" id="female" name="gender">
                </p>
                <p>
                    <label  for="other">Other</label><br>
                    <input type="radio" id="other" name="gender" checked="checked">
                </p>
            </form>

            <h3>Payment method</h3>
            <p>
                <label for="payment">Please chose one: </label><br>
                <select id="payment" name="pay">
                    <option selected="selected">Card (debit/credit)</option>
                    <option>Apple Pay</option>
                    <option>Swish</option>
                    <option>Bank transfer</option>
                </select>
            </p>
            <button type="submit">
                <img src="/img/delivery.png" style="width: 40px">
                Place my order!
            </button>
        </section>

        <section>
            <h3>Customer information</h3>
            <p>This is where you supply provide nice information</p>
        </section>
    </main>
    <hr>
    <footer>
        &copy; Insane Burgers 2026
    </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");

function MenuItem(name, url, kCal, lactose, gluten) {
  this.name = name;
  this.url = url;
  this.kCal = kCal;
  this.lactose = lactose;
  this.gluten = gluten;

}

const ListOfBurgers = [
  new MenuItem("Regular burger", "/img/regular_burger.jpg", 500, true, true),
  new MenuItem("Ugly burger", "/img/ugly_burger.jpg", 5000, true, true),
  new MenuItem("Golden burger", "/img/golden_burger.jpg", 0, false, false),
  new MenuItem("BBQ-Bacon burger", "/img/bbq_burger.jpg", 700, false, true)
];
console.log(ListOfBurgers);

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: ListOfBurgers
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Google+Sans+Flex:opsz,wght@1..144,1..1000&display=swap');
body {
    font-family: "Google Sans Flex", sans-serif;
    font-style: normal;
    font-optical-sizing: auto;
    font-size: 12pt;
}

p {
    color: black;
}

h1 {
    font-family: "Bebas Neue", sans-serif;
    font-size: 36pt;
    letter-spacing: 0.05em;
}
main, header, footer, nav ul {
    max-width: 80rem;
    margin: 0 auto 0 auto;
}
main {
    background-color: bisque;
}

section {
    margin: 20px;
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */

header h1 {
    position: absolute;
    top: 60px;
    left: 20px;
    width:40rem;
    margin: 0 auto;
    text-align: center;

    text-shadow: -2px 2px 2px red;
}

header {
    overflow: hidden;
    width: 100%;
    height: 200px;
    position: relative;
}

header img {
    width: 100%;
    height: 200px;
    opacity: 0.7;
}
nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

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
    justify-items: center;
}
#contact {
    border: 4px dotted black;
    padding: 10px;
}
button:hover {
    background-color: lightblue;
    cursor: pointer;
}
input[type="radio"]:hover, select:hover {
    cursor: pointer;
}

button {
    margin: 20px 0;
}

</style>