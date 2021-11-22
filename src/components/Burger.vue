<template>
  <div class="wrapper">
    <Burger v-for ="burger in BurgerArray"
            v-bind:burger="burger"
            v-bind:key="burger.name"
            v-bind:src="burger.image"/>


      <div class="box">
        <h2>{{ burger.name }}</h2>
        <img v-bind:src="burger.image" style="width: 200px;height:200px">
        <ul>
          <li>Kött</li>
          <li v-if="burger.gluten"> Innehåller <span class ="allergener">gluten</span></li>
          <li v-if="burger.lactose"> Innehåller <span class ="allergener">laktos</span></li>
          <li> {{burger.calories}} kalorier</li>
        </ul>

        <div>
          <button class = "burgerButtons" v-on:click="increaseBurgers">
            <img id = "plusImage" src="https://cdn.icon-icons.com/icons2/2550/PNG/512/plus_icon_152556.png">
          </button>

            <button class="burgerButtons" v-on:click="decreaseBurgers">
              <img id = "minusImage" src="https://cdn0.iconfinder.com/data/icons/typicons-2/24/minus-1024.png">
            </button>

        <p>antal: {{amountOrdered}}</p>
     </div>
    </div>

  </div>

</template>



<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    increaseBurgers: function() {
      this.amountOrdered++;
      this.$emit('orderedBurger', { name:   this.burger.name,
            amount: this.amountOrdered
          }
      );
    },
    decreaseBurgers: function(){
      if(this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('orderedBurger', { name:   this.burger.name,
              amount: this.amountOrdered
            }
        );
      }
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.allergener {
  font-weight: bold;
}


.wrapper {
  display: grid;
  grid-template-columns: 400px 400px 400px;
}

.box {
  padding: 20px;
  margin: 30px;
}

.burgerButtons{
}
#plusImage{
  height:20px;
  width:20px;
  background-color:green;
}
#minusImage{
  height:20px;
  width:20px;
  background-color:darkred;
}

</style>
