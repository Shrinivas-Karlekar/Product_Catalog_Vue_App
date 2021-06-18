<template>
  <div class="container">
    <h1>Product Catalog with cart</h1>
    <div>
      <Products></Products>
    </div>
  </div>
</template>

<script>
import Products from "./components/Products";
export default {
  name: "App",
  components: {
    Products
  }
}
</script>

<style>
</style>
