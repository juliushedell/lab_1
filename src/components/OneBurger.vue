<template>
  <div class="burger-item">
    <h3>{{ burger.name }}</h3>
    <img v-bind:src="burger.url" class="images">
    <ul>
      <span class="ingredients">
        <li>Calories: {{ burger.kCal }}</li>
        <li>{{ checkGluten(burger.gluten) }}</li>
        <li>{{ checkLactose(burger.lactose) }}</li>
      </span>
    </ul>
    <div id="buttons">
      <button v-on:click="decrease">-</button>
    {{ amountOrdered }}
    <button v-on:click="increase">+</button>
    <button v-on:click="addBurger">add to basket</button>
    </div>
  </div>
</template>
  
  <script>

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
    checkGluten: function(gluten) {
      if (gluten) {
        return "Contains gluten"
      }
      else {
        return "Gluten free"
      }
    },
    checkLactose: function(lactose) {
      if (lactose) {
        return "Contains lactose"
      }
      else {
        return "Lactose free"
      }
    },
    increase: function() {
      this.amountOrdered += 1;
    },
    decrease: function() {
      if (this.amountOrdered != 0) {
        this.amountOrdered -= 1;
      }
    },
    addBurger: function () {
  this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
    );
    this.amountOrdered = 0;
  },
  }
}
  
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>


.images {
  height: 15em;
}

h3 {
  text-align: center;
}

#buttons {
  margin-left: 1.5em;
}


/*
.ingredients {
    color: #ff0000;
    font-weight: bold;
}
*/
  </style>
  