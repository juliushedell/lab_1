<template>
  <header>
    <img src="img/restaurant_ny.jpg" id="headpicture">
    <h1>Welcome to Blasted Burgers!</h1>
  </header>
  <main>
    <h3 id="menu">MENU</h3>
    <p id="instructions">Here you can see the amazing burgers we provide.</p>
    <section class="burgers">
      <div class="wrapper">
        <Burger v-for="burger in burgers" 
          v-bind:burger="burger" 
          v-bind:key="burger.name"
          v-on:orderedBurger="addToOrder($event)" />
      </div>
    </section>
    <section id="contact">
      <h3>
        Order information:
      </h3>
      <h4>
        Basket:
      </h4>
      <div>
        {{ orderedBurgers }}
      </div>
      <form>
        <p>
          <label for="fullname">Name</label><br>
          <input type="text" id="fullname" v-model="fn" required="required" placeholder="Full name">
        </p>
        <p>
          <label for="email">E-mail</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail adress">
        </p>
        <div id="mapContainer">
          <div id="map" v-on:click="addOrder">
          click here
          <div id="dots">
          <div v-bind:style="{left: location.x + 'px', top: location.y + 'px'}">
            T
          </div>
        </div>
        </div>
        </div>
        <p>
          <label for="recipient">Payment method</label><br>
          <select id="recipient" v-model="rcp">
            <option>Debit card</option>
            <option>Credit card</option>
            <option selected="selected">Swish</option>
            <option>Klarna</option>
            <option>Paypal</option>
          </select>
        </p>
        <p>
          Please select gender<br>
          <label>
            <input type="radio" v-model="gender" value="Male"> Male
          </label><br>
          <label>
            <input type="radio" v-model="gender" value="Female"> Female
          </label><br>
          <label>
            <input type="radio" v-model="gender" value="Do not wish to provide" checked="checked"> Do not wish to provide
          </label>
        </p>
      </form>
    </section>
    <button id="sendButton" type="submit" v-on:click="markDone">
      <img src="img/Delivery.png" style="width: 1cm;">
      Place order!
    </button>
  </main>
  <hr>
  <footer>
    <p id="footer"> &copy; 2023 Blasted Burgers! All rights reserved. </p>
  </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

/*
function menuItem(nm, kc, url, gl, la){
  this.name = nm;
  this.kCal = kc;
  this.url = url;
  this.gluten = gl;
  this.lactose = la;
 }
 */

/* let burgersall = [
  {name: "Fattypatty", kCal: 500, url:"img/Burger_1.jpeg", lactose: true, gluten: true}, 
  {name: "American style", kCal: 1000, url: "img/Burger_2.jpeg", lactose: false, gluten: true},
  {name: "Crispy chicken", kCal: 700, url: "img/Burger_4.jpeg", lactose: false, gluten: false}];

  */

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      fn: '',
      em: '',
      ad: '',
      ho: '',
      rcp: '',
      gender: '',
      orderedBurgers: {},
      location: { x: 0,
                  y: 0
                }
    }
  },
  methods: {
    markDone: function () {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y + 20
        },

        infoCustomer: {
          name: this.fn,
          email: this.em,
          paymentMethod: this.rcp,
          gender: this.gender
        },
        orderItems: this.orderedBurgers
      }
      );
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers)
    },
    setLocation: function(event) {
      this.location.x = event.clientX - 10 - event.currentTarget.getBoundingClientRect().left;
      this.location.y = event.clientY - 30 - event.currentTarget.getBoundingClientRect().top;
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 30 - offset.y;
    }
  }
}

</script>

<style>
body {
  background-color: rgb(189, 201, 212);
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

header,
main {
  margin: 2em;
}

#contact {
  background-color: black;
  color: white;
  border: white;
  border-style: dashed;
  padding: 1em;
}

button:hover {
  background-color: rgb(0, 98, 255);
  color: white;
  cursor: pointer;
}

#sendButton {
  margin-top: 1em;
  margin-left: 1em;
}

section {
  margin: 0.3em;
}

#footer {
  font-size: small;
}

#headpicture {
  width: 100%;
  opacity: 0.3;
}

h1 {
  position: absolute;
  left: 0;
  top: 0;
  margin-top: 5em;
  margin-left: 14em;
}

.burgers {
  border: black;
  border-style: dashed;
}

.wrapper {
  padding: 1em;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  max-width: 100%;
}

#menu, #instructions {
  text-align: center;
}

#map {
  background-image: url("../../public/img/polacks.jpg");
  background-size: cover;
  width: 1920px;
  height: 1078px;
}

#mapContainer {
  overflow: scroll;
  height: 500px;
  width: 100%;
}

#dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

</style>
