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
              v-bind:key="burger.name"
              v-bind:burger="burger"
              v-on:orderedBurgers="addToOrder($event)"
            />
        </div>
        </section>

        <section id="contact">
            <h2>Delivery information</h2>
            <form id="orderForm">
                <p>
                    <label for="fullName">Full name</label><br>
                    <input type="text" id="fullName" v-model="fn" required="required" placeholder="First- and Last name">
                </p>
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                </p>
                <h3>Please select your gender</h3>
                <p>
                    <label  for="male">Male</label><br>
                    <input type="radio" id="male" value="Male" v-model="gender">
                </p>
                <p>
                    <label  for="female">Female</label><br>
                    <input type="radio" id="female" value="Female" v-model="gender">
                </p>
                <p>
                    <label  for="other">Other</label><br>
                    <input type="radio" id="other" value="Other" v-model="gender">
                </p>
            

            <h3>Payment method</h3>
            <p>
                <label for="payment">Please chose one: </label><br>
                <select id="payment" v-model="pay">
                    <option selected="selected">Card</option>
                    <option>Apple Pay</option>
                    <option>Swish</option>
                    <option>Bank transfer</option>
                </select>
            </p>
            
            <h4>
                Please select delivery location on the map.
            </h4>
            <div class="map-container">
                <div id="map" v-on:click="setLocation">
                    <div id="target" 
                    v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}">
                    T
                    </div>
                </div>
            </div>

            <h4>
                All done?
            </h4>

            <button type="submit" v-on:click="placeOrder()">
                <img src="/img/delivery.png" style="width: 40px">
                Place my order!
            </button>
            </form>

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
import menu from '../assets/menu.json'
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
      burgers: menu,
      fn: "",
      em: "",
      gender: "Other",
      pay: "Card",

      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      },

    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    /*addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
        };
        this.location.x = event.clientX - 10 - offset.x;
        this.location.y = event.clientY - 10 - offset.y;

        
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
                 /*alert("Order placed!")*
    },*/
    setLocation: function (event) {
        var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
        };
        this.location.x = event.clientX - 10 - offset.x;
        this.location.y = event.clientY - 10 - offset.y;

    },

    addToOrder: function (event) {
        this.orderedBurgers[event.name] = event.amount;
    },

    placeOrder: function (event) {
        console.log("Name:", this.fn);
        console.log("E-mail:", this.em);
        console.log("Gender:", this.gender);
        console.log("Payment method:", this.pay);
        console.log("Ordered burgers:", this.orderedBurgers);

        socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                           fn: this.fn,
                                           em: this.em,
                                           gender: this.gender,
                                           pay: this.pay,
                                        },
                                orderItems: this.orderedBurgers,
                              }
                 );
                 alert("Order placed!")
    },

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
    max-width: calc(80rem - 40px);
    padding: 20px;
    margin-top: 20px;
}

section {
    margin: 0 0 20px 0;
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
    padding: 10px 10px 30px 10px;
    border: 4px dotted white;
}

#burgers p {
    color: white;
}

.burger-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-row-gap: 40px;
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

#map {
    background: url("/img/polacks.jpg");
    width: 1920px;
    height: 1078px;
    position: relative;
  }
.map-container {
    width: 100%;
    height: 700px;
    overflow: scroll;
}
.map-container:hover {
    cursor: crosshair;
}

#target {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
</style>