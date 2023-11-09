<template>
  <h4>{{ burger.name }}</h4>
    <img v-bind:src="burger.url" class="images">
      <ul>
        <span class="ingredients">
          <li>{{ burger.kCal }}</li>
          <li>{{ burger.gluten }}</li>
          <li>{{ burger.lactose }}</li>
        </span>
      </ul>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

.images {
  height: 15em;
}

burger.name {
    text-transform: uppercase;
    text-align: center;
}

.ingredients {
    color: #ff0000;
    font-weight: bold;
}
  </style>
  