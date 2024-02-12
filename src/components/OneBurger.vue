<template>

  <section class="bgitem"> 

              <h3>{{burger.name}}</h3>
          <img v-bind:src="burger.img" alt="No burgir :(">

          <ul>
            <li v-for="x in burger.contains">
              {{x}}
            </li>
          </ul>

          {{amountOrdered}}
          <br>
          <button v-on:click="add">+</button> <button v-on:click="sub">-</button>

          </section>

  </template>
  
  <script>
    import menu from '../assets/menu.json'


  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data: function () {
      return {
        amountOrdered: 0,
      }
    },
    methods: {
      add: function () {
        this.amountOrdered +=1;
        this.$emit('orderedBurger', {name: this.burger.name,
          amount: this.amountOrdered})
      },
      sub: function () {
        this.amountOrdered -= 1;
        this.$emit('orderedBurger', {name: this.burger.name,
          amount: this.amountOrdered})
      },

    }

  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
.gridw {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 30% 30% 30%;
  background-color: #000000;
  color: #ffffff;

}

.bgitem img {
  height: 170px

}

.bgitem {
  display: inline-block;
  padding: 1em 0em;
  margin: 1em 0.5em;
}


.grid1 {
  grid-column: 1
}

.grid2 {
  grid-column: 2
}

.grid3 {
  grid-column: 3
}

  </style>
  