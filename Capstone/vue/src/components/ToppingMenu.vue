<template>
  <div id="topping-menu">

   
<select class="form-control" name="template" v-model="selectedTopping" multiple size="24" style="height: 100%;">
    <option v-for="topping in toppings" v-bind:key="topping.toppingName" v-on:click="addTopping()">
       {{ topping.toppingName }} -- ${{ formatPrice(topping.additionalPrice) }}
    </option>
</select>
    <div id="topping-display" v-if="selectedTopping !== null" v-bind:key="selectedTopping.toppingName">
      <b>You're Adding</b>: {{ selectedToppings }}
  </div>
   <button type="submit">Add Toppings To Pizza</button>

   <button type="submit" v-on:click="setToppingsToCart()">Confirm Choice</button>
</div>
</template>

<script>
import toppingService from "@/services/ToppingService.js";

export default {
  name: "topping-menu",
  data() {
    return {
      selectedTopping: {
      },
    };
  },
  computed: {
    toppings() {
      return this.$store.state.toppings;
    },
    selectedToppings() {
      return this.$store.state.selectedToppings;
    },
    toppingCart() {
        return this.$store.state.toppingCart;
    }
  },
  methods: {
    listTopping() {
      toppingService.listTopping().then((response) => {
        this.$store.commit("SET_TOPPING", response.data);
      });
    },
    addTopping() {
      let topping = this.selectedTopping;
      this.$store.commit("SET_SELECTED_TOPPINGS", topping);
    },
     formatPrice(additionalPrice) {
        let val = (additionalPrice/1).toFixed(2).replace('.', '.')
        return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    },
    setToppingsToCart() {
        let selectedToppings = this.selectedToppings;
        if (this.selectedToppings.length > 0) {
            selectedToppings.forEach(topping => {
                this.$store.commit("SET_TOPPING_CART", topping);
            });
        }

    },
     emptyToppingCart() {
      let emptyToppingCart = [];
      this.$store.commit("CLEAR_TOPPING_CART", emptyToppingCart);
    },
  },
  
  created() {
    this.listTopping();
  },
};
</script>


<style>

#form-control {
    margin: auto;
    padding: auto;
}


</style>