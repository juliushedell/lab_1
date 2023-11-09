<template>
  <header>
         <img src="img/restaurant_ny.jpg" id="headpicture">
         <h1>Welcome to Bashkan's Burgers!</h1>
      </header>
      <main>
         <h3 id="menu">MENU</h3>
         <p id="instructions">Here you can see the amazing burgers we provide.</p>
         <section class="burgers">
          <div>
          <Burger v-for="burger in burgersall"
              v-bind:burger="burger" 
              v-bind:key="burger.name"/>
            </div>
         </section>
         <section id="contact">
            <h3>
               Order information:
            </h3>
            <form>
               <p>
                  <label for="fullname">Name</label><br>
                  <input type="text" id="fullname" name="fn" required="required" placeholder="Full name">
               </p>
               <p>
                  <label for="email">E-mail</label><br>
                  <input type="email" id="email" name="em" required="required" placeholder="E-mail adress">
               </p>
               <p>
                  <label for="adress">Street</label><br>
                  <input type="text" id="address" name="ad" required="required" placeholder="Street name">
               </p>
               <p>
                  <label for="house">House</label><br>
                  <input type="number" id="house" name="ho" required="required" placeholder="House number">
               </p>
               <p>
                  <label for="recipient">Payment method</label><br>
                  <select id="recipient" name="rcp">
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
                  <input type="radio" name="gender" value="Male"> Male
                  </label><br>
                  <label>
                  <input type="radio" name="gender" value="Female"> Female
                  </label><br>
                  <label>
                  <input type="radio" name="gender" value="Do not wish to provide" checked="checked"> Do not wish to provide
                  </label>
               </p>
            </form>
         </section>
         <button type="submit">
         <img src="img/Delivery.png" style="width: 1cm;">
         Place order!
         </button>
      </main>
      <hr>
      <footer>
         <p id="footer"> &copy; 2023 Bashkan's Burgers! All rights reserved. </p>
      </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();

function menuItem(nm, kc, url, gl, la){
  this.name = nm;
  this.kCal = kc;
  this.url = url;
  this.gluten = gl;
  this.lactose = la;
 }

let burgersall = [
  {name: "FATTYPATTY", kCal: 500, url:"img/Burger_1.jpeg", lactose: true, gluten: true}, 
  {name: "American style", kCal: 1000, url: "img/Burger_2.jpeg", lactose: false, gluten: true},
  {name: "EUW", kCal: 700, url: "img/Burger_3.jpeg", lactose: false, gluten: false}];

console.log(burgersall)

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgersall
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
body {
    background-color:rgb(189, 201, 212);
    font-family:Verdana, Geneva, Tahoma, sans-serif;
}
/* ALLERGY INFORMATION*/
.ingredients {
    color: #ff0000;
    font-weight: bold;
}

#burgernames {
    text-transform: uppercase;
    text-align: center;
}

header, main {
    margin: 2em;
}

#menu, #instructions {
    text-align: center;
}

.burgers {
    border: #ff0000;
    border-style: dashed;
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

button {
    margin-top: 2em;
    margin-left: 0.8em;
}

.images {
    height: 15em;
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

.wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(3, 1fr);
}

.box {
    border-radius: 5px;
    padding: 10px;
    margin: auto;
}
</style>