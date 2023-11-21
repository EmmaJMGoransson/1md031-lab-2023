<template>
  <header> 
    <h1> Brilliant Burgers </h1>
      <img src="img/earth.jpeg" id="headerimg">
    </header>
    <main>
      <section id="menu"> 
        <h1 id="headerMenu"> Our Burgers </h1>
        <h3 id="headerMenu" > Below you can see our selection of brilliant burgers</h3>
        <hr>
        <div class="wrapper">
             <!-- loopen som skriver ut burgare med bild  -->
              <Burger v-for = "burger in burgers"
                      v-bind:burger="burger" 
                      v-bind:key="burger.name"
                      v-on:orderedBurger="addToOrder($event)"/>
        </div>
            </section>
            <section id="contact">
                <form>
                    <h2>Delivery form</h2>
                    <p>Fill out the for below to provide us with the information we need in order to deliver your order</p>
                    <hr>
                    <p>
                        <label for="firstname">First name: </label><br>
                        <input type="text" id="firstname" v-model="fn" required="required" placeholder="Firstname">
                    </p>
                    <p>
                        <label for="lastname">Last name: </label><br>
                        <input type="text" id="lastname" v-model="ln" required="required" placeholder="Lastname">
                    </p>
                    <p>
                        <label for="email"> E-mail: </label><br>
                        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                    </p>
                    <h3>Select gender: </h3>
                    <label> 
                        <input type="radio" v-model="gender" value="female"> Female
                    </label>
                    <label>
                        <input type="radio" v-model="gender" value="male"> Male
                    </label>
                    <label>
                        <input type="radio" v-model="gender" value="nonbinary"> Non binary
                    </label>
                    <label>
                        <input type="radio" v-model="gender" value="dontwannasay"> Don't wanna say
                    </label>
                    <div>
                      <h3>Select your drop of location below: </h3>
                    </div>
                    <div class="wrapmap">
                      <div id="map" v-on:click="setLocation($event)"> 
                        <div id="Ts">
                          <div v-bind:style="{ left: location.x +'px', 
                                              top: location.y + 'px' }">
                            <div>
                              T
                            </div>
                          </div> 
                        </div>
                      </div> 
                    </div>
                    <h3>Payment method</h3>
                    <p>
                      <label for="paymentmethod">Choose payment method: </label> 
                      <select id="recipient" v-model="rcp">
                          <option>Swish</option>
                          <option>Klarna</option>
                          <option>Debit Card</option>
                          <option>Credit Card</option>
                      </select>
                    </p>
                </form>
                  <div id="orders">
                    <button  v-on:click="markDone(key)">
                      <img src="img/fagel.jpeg" style="width: 60px;"> 
                      <h3>Place order </h3> 
                    </button>
                  </div>  
            </section>
        </main>
        <hr> 
        <footer>
            <p> &copy; 2023 Emma Göransson</p>
            <p> Mail: brilliant@burger.com </p>
            <p> Phone +46712345678</p>
            <p> Adress: Brilliant Boulevard 101 A</p>
        </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json' 

const socket = io();

/*function MenuItem (name, url, contents1, contents2, contents3, allergens) {
  this.name = name; // The *this* keyword refers to the object itself
  this.url = url;
  this.contents1 = contents1; 
  this.contents2 = contents2;
  this.contents3 = contents3;
  this.allergens = allergens;  
} */
/* let Burgare = [
  {name: "Black Burger", contents1: "Ground beef aka dead cow", contents2: "Severely burnt bread", contents3: "1/2 kg pepper", url: "img/Black Burger.jpeg", allergens: "lactose, disapointment"},
  {name: "Green Burger", contents1: "chickpea patty, dryer than our sense of humor", contents2: "Spinatch bread", contents3: "54 pickles", url: "img/Green burger.jpeg", allergens: "gluten"},
  {name: "White Burger", contents1: "Uncooked chicken", contents2: "Unbaked bread", contents3: "Ginger (not the human kind)", url: "img/White burger.jpeg", allergens: "salmonella" }] */

  /*console.log(MenuItem);*/

export default {
  name: 'HomeView',
  components: {
    Burger
  },
      data: function () {
    return {
      burgers: menu,
      gender: "", 
      fn: "",
      ln: "", 
      em: "",  
      rcp: "",
      orders:[],
      orderedBurgers: {},
      location: { x: 0,
                 y: 0 }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    markDone: function() {
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                          y: this.location.y},
                                orderItems: this.orderedBurgers,
                                custumerInfo: {fn:this.fn, 
                                              ln:this.ln, 
                                              em:this.em, 
                                              gender:this.gender, 
                                              rcp:this.rcp}
                                }
                    );
    },
   addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x
      this.location.y = event.clientY - 10 - offset.y

    }, 
    setLocation: function(event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x
      this.location.y = event.clientY - 10 - offset.y
   },
   addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers)
    } 
  }
}
</script>

<style>
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';


body {
    background-color: lightpink;
    font-family: 'Times New Roman', Times, serif;
    color:seagreen; 
    font-weight: bold;
    font-size: medium;
    margin-top: 50px;
    margin-bottom: 50px; 
    padding: 20px; 
 }

 .contents {
    color:pink
 }

 #allergens{
    text-transform: uppercase; 
 }

 #contact{
    background-color: black;
    color: white;
    border: 5px dashed powderblue;
    padding: 50px;
    margin-top: 50px; 
    margin-bottom: 50px;
}

 button:hover {
    cursor: pointer; 
    background-color: rgb(96, 175, 96);
 }

 #headerMenu{
  color: rgb(132, 203, 212); 

 }

 #menu{ 
    background-color:beige; 
    border: 5px dotted powderblue; 
    padding: 50px;
 }

header{
    color: white;
    border: 10px double white ;
    margin-top: 50px; 
    margin-bottom: 50px;
    padding: 50px;
    text-align: center;
    position:relative; 
    overflow:hidden; 
 }

#headerimg{
    opacity: 0.7;    
    position: absolute; 
    width: 100%;
    height :100%; 
    top: 0; 
    left: 0;
    z-index: -1;
}

 button{
    margin: 20px;
 }

 .wrapper {
    display: grid; 
    grid-gap: 10px;
    grid-template-columns: repeat(3, 1fr);
}

.box {
    border-radius: 5px;
    padding: 20px;
}

  #map {
    height: 1078px;
    width: 1920px;
    background: url("../../public/img/polacks.jpg");
  }

  .wrapmap{
    height: 500px;
    width: 600px;
    overflow: scroll;

 }

  #Ts {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }

  #Ts div{
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
    position: absolute;
  }

</style>