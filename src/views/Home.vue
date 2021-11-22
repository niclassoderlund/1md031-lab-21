<template>
  <header>
    <img src="https://static.thatsup.co/content/img/place/t/u/tugg-burgers-uppsala-0.jpg">
    <h1>Välkommen till BurgerOnline</h1>
  </header>
  <main>

     <section id="Burgersection">
      <h2>select burger</h2>
      <p> click your desired burger </p>

      <div class="wrapper">
        <Burger v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>
        </div>



    </section>


    <section id="contact" style="clear:left">
      <h2>Costumer info</h2>
      <p> this is where you enter info about you </p>

      <h2>Delivery info</h2>
      <form>
        <p>
          <label for="firstname">Full name</label><br>
          <input type="text" id="firstname" v-model="fn" required="required" placeholder="first- and last name">
        </p>
        <p>
          <label for="email">E-mail</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
        </p>
        <!--
        <p>
          <label for="street">Street</label><br>
          <input type="text" id="street" v-model="st" placeholder="Street">
        </p>
        <p>
          <label for="house">House</label><br>
          <input type="number" id="house" v-model="hs" placeholder="House">
        </p>
        -->
        <p>
          <label for="payment">Payment method</label><br>
          <select id="payment" v-model="pmt">
            <option>Swish</option>
            <option>Credit card</option>
            <option>Klarna</option>
            <option>Bank transaction</option>
          </select>
        </p>



        <label for="male">Gender info</label> <br>
        <input type="radio" id="male" v-model="gender" value="male"> Male <br>
        <input type="radio" id="female" v-model="gender" value="female"> Female <br>
        <input type="radio" id="do not wish to provide" v-model="gender" value="do not wish to provide"> Do not wish to provide

      </form>
      <br>
      <br>

      <div id="mapHolder">
        <h3> click your current location</h3>
        <div id="map" v-on:click="setLocation">
          <div id="dots">
            <div v-bind:style="{ left: location.x + 'px',
            top: location.y + 'px' }">
              T
            </div>
          </div>
        </div>
      </div>



    </section>
    <br>
    <br>
    <button v-on:click=OrderDone type="submit">
      <img src="data:image/png;base64,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" style="width: 15px; height:15px">
      Send Info
    </button>
  </main>
  <hr>
  <footer>
    &copy; SoderlundBurgerCO
  </footer>

</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();


/*function MenuItem(name, URL, cal, gluten, lactose) {
  this.name = name; // The *this* keyword refers to the object itself
  this.image = URL;
  this.calories = cal;
  this.gluten = gluten;
  this.lactose = lactose;
}*/
const BurgerArray = menu

    /*[new MenuItem("burgir","https://i.ytimg.com/vi/grZI4digmm0/sddefault.jpg",500,true,true),
  new MenuItem("bomben", "https://thumb.mp-farm.com/7197109/preview.jpg", 700, true, true ),
  new MenuItem("mini", "https://imageresizer.static9.net.au/1cEo1pjkilCsUSERF8TzeJUfL6Y=/1200x628/smart/https%3A%2F%2Fprod.static9.net.au%2F_%2Fmedia%2F2017%2F09%2F18%2F11%2F06%2FJimmy-Hurlstons-mini-burger-with-mac-and-cheese.jpg", 350, true, true)]*/




export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: BurgerArray,
      fn: "",
      em: "",
      st: "",
      hs: "",
      pmt: "Swish",
      gender: "do not wish to provide",
      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },

   /*
    addOrder: function (event) {
      console.log(this.fn)
      console.log(this.em)
      console.log(this.st)
      console.log(this.gender)
      console.log(this.pmt)
      console.log(this.hs)
      console.log(this.orderedBurgers)
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      console.log(this.location.x = event.clientX - 10 - offset.x, this.location.y = event.clientY - 10 - offset.y,),

          socket.emit("addOrder", {
                orderId: this.getOrderNumber(),
                details: {
                  x: event.clientX - 10 - offset.x,
                  y: event.clientY - 10 - offset.y
                },
                orderItems: ["Beans", "Curry"]
              }
          );


    },
*/

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

    OrderDone: function () {
      console.log(this.em, this.fn, this.pmt, this.gender, this.orderedBurgers)
      socket.emit("addOrder", {
            orderId: this.getOrderNumber(),
            details: {
              x: this.location.x, y: this.location.y,
              fn: this.fn, em: this.em, pmt: this.pmt, gender: this.gender
            },
            orderItems: [this.orderedBurgers]
          }
      );
    },
  }
}
</script>

<style>

 #mapHolder{
  height: 500px;
  width: 700px;
  overflow:scroll;
}

  #map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
  }

  header {
    margin: 5px 10px;
    height: 500px;
    overflow:hidden
  }

  header h1 {
    position: absolute;
    padding: 20px;
    margin-top: -800px;
  }

  header img {
    opacity: 0.7;
    width: 100%;
    height: auto;
  }

  body {
    font-family: "Times New Roman", sans-serif;
    font-size: 14pt;
  }
  .allergener {
    font-weight: bold;
  }



  section {
    padding: 20px;
    margin: 20px;
  }

  #Burgersection {
    background-color:black;
    color: white;
    margin: 10px 10px;
    border: 2px dashed #ffffff;
  }

  #contact {
    margin: 50px 10px;
    border: 2px dashed #050505;
  }

 #dots div {
   position: absolute;
   background: black;
   color: whitesmoke;
   border-radius: 10px;
   width:20px;
   height:20px;
   text-align: center;
 }

 #dots {
   position: relative;
   margin: 0;
   padding: 0;
   background: url(/img/polacks.jpg);
   background-repeat: no-repeat;
   width:1920px;
   height: 1078px;
   cursor: crosshair;
 }

  button:hover {
    color: green;
  }

  a[href], input[type='submit'], input[type='image'], label[for], select, button, pointer {
    cursor: pointer;
  }

  .wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 400px 400px 400px;
  }

</style>
