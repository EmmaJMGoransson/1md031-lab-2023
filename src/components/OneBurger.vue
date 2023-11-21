<template>
 <div>
  <h3> {{burger.name}}</h3>
                      <img v-bind:src="burger.url" style="width: 200px; height:140px" alt={{burger.name}}>
                      <ul>
                          <section class="contents">
                          <li>{{ burger.contents1 }}</li>
                          <li>{{burger.contents2}}</li>
                          <li> {{burger.contents3}}</li>
                          <li>Contains: <span id="allergens"> {{burger.allergens}}</span></li>  
                          </section>
                      </ul>
                      <p>{{ burger.amountOrdered }}</p>
  <button @click="removeBurger">
    -
  </button>
  <span>{{amountOrdered}}</span>
  <button @click="addBurger">
    +
  </button>
</div>
</template>
  
  <script>
  //import { methods } from 'http';

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
    removeBurger: function() {
      if (this.amountOrdered > 0 ){
      this.amountOrdered --;
      this.$emit('orderedBurger', {name: this.burger.name,
                                    amount: this.amountOrdered
                                  }
        );
                                }
      },
    addBurger: function() {
      this.amountOrdered ++;
      this.$emit('orderedBurger', {name: this.burger.name,
                                  amount: this.amountOrdered
                                }
      );
    }
  }
}

  </script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  </style>
  