<template>



  <head>
       <title>BURGIE</title>
       <meta charset="utf-8"/>
   </head>
   <body>
    <link rel="stylesheet" type="text/css" href="/style.css">

    <header>
      

      
      <img src="https://gizmodo.com.au/wp-content/uploads/2021/02/19/opgj43no5dqhgafhypuk.gif?quality=75&w=640&h=360&crop=1" class="himg">

      <h1 class="hh1">Welcom to burgirs burier</h1>




    </header>

    <main>

        <div>
    <div>
      <h1>Burgers</h1>

      <p>choose <span class="bg">burgir</span> </p>

      <section class="gridw">
      <Burger class="gridw" v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>

              </section>
    </div>

  </div>



        <section>

        


        <form id="info">

          <h3>About you</h3>


        <p>
          <label for="name">Name</label>
          <br>
          <input id="name" type="text" v-model="name" required="required" placeholder="full name"> 
        </p>        
        <p>
          <label for="mail">Mail</label>
          <br>

          <input id="mail" type="email" v-model="mail" required="required" placeholder="email">
        </p>


        <p>
          <label for="sacrifice">How to pay</label> <br>
          <select id="sacrifice" v-model="currency" required="required">
            <option>V-bucks</option>
            <option>Robux</option>
            <option>B-B-B-BITCOIN</option>
            <option selected="selected">Cash</option>
          </select>
        </p>

        <p>
          <label for="helicopter">Helicopter</label>
          <input name="gender" id="helicopter" type="radio" v-model="input" value="Helicopter">
          <br>

          <label  for="mm">mailman</label>
          <input name="gender" id="mm" type="radio" v-model="input" value="mailman">
          <br>

          <label for="pz">panzerkampfvagen VI tiger</label>
          <input name="gender" checked="checked" id="pz" type="radio" v-model="input" value="panzerkampfvagen VI tiger">
        </p>


        


        </form>

        <section id="container">

            <div id="map" v-on:click="addOrder">
      <div id = "dot" v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}">
            T
          </div>
    </div>

    </section>

        <button v-on:click="sendLocation" form="info">
          <img src="https://i.pinimg.com/originals/f8/32/73/f832737ef4ac261c5e8c5da90c57deb3.png" style="height: 100px;" >
          <br>
          send
        </button>


        </section>








    </main>


    <footer>
      <hr>

      <section>


        
        <img src="https://cdn.openart.ai/stable_diffusion/892728f0f39ec21eb93f1ad07e0d80c9ab2e952d_2000x2000.webp" alt="No cookies :(" style="width: 30%;">


        <p>cookies?</p>
        &#169;

      </section>
      


      
    

      
    </footer>
  </body>


</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'



const socket = io();

function menueItem(n, imgURL, contains) {
  this.name = n;
  this.URL = imgURL;
  this.contains = contains;

}


const burgerArray = menu







export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgerArray,
      name: "",
      mail: "",
      street: "",
      hn: "",
      currency: "",
      input: "panzerkampfvagen VI tiger",
      orderedBurger: {},
      location: { x:0,
                  y:0}

    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    addOrder: function (event) {

      var offset = {
      	x:event.currentTarget.getBoundingClientRect().left,
      	y:event.currentTarget.getBoundingClientRect().top
      }
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
      
      
    },


    sendLocation: function(event) {

    	if (this.name != "" && this.mail != "" && this.currency != "") {
    		socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                           name: this.name,
                                           mail: this.mail,
                                           currency: this.currency,
                                           gender: this.input },
                                orderItems: this.orderedBurger
                              }
                 );
    	}



    },
    clicked: function(formElements) {
      console.log("clicked")
    },
    addToOrder: function (event) {
      this.orderedBurger[event.name] = event.amount;
    },
  }
}
</script>

<style>
  #map {
    position: relative;
    background-repeat: no-repeat;
    width: 1920px;
    height: 1078px;
    cursor: crosshair;
    background: url("../../public/img/polacks.jpg");
    margin: 0;
    padding: 0;
  },

  #map div {
  	position: absolute;
  	background: black;
  	color: white;
  	border-radius: 10px;
  	width: 20px;
  	height: 20px;
  	text-align: center;
  }

  #container {
    width: 100%;
    overflow: scroll;

    height: 500px;
  }
  #dot {
  	position: absolute;
  	color: black;
  	text-align: center;

  }
</style>