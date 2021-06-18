<template>
  <div>
    <div id="btnContainer">
      <button class="btn compare" v-on:click="showProducts()">Show Products Page</button>
    </div>
    <div v-for="product in products" v-bind:key="product.title">
      <div class="card border-primary mb-3" style="max-width: 18rem;">
        <div class="card-header">{{ product }}</div>
        <button @click="removeFromCart(product)">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CompareProducts",
  props: ['products', 'productsAddedToCart'],
  methods: {
    showProducts() {
      this.$emit('backToProductsView');
    },
    removeFromCart(product) {
      const index = this.productsAddedToCart.indexOf(product.title);
      if (index > -1) {
        console.log('');
      }
    }
  }
}
</script>

<style scoped>

</style>