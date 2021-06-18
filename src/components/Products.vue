<template>
  <div>
    <div v-if="!compareProductsView">
      <div id="btnContainer">
        <button class="btn" v-on:click="changeView()">Change View</button>
        <button class="btn compare" v-on:click="compareProducts()">Compare Products</button>
      </div>
      <div>

      </div>
      <div v-if="displayWarning">
        <div class="alert">
          <span class="close-btn" onclick="this.parentElement.style.display='none';"></span>
          <strong>Warning!</strong>{{ productsAvailable }} Items in stock remaining for the {{ productName }}.
        </div>
        <div>Your Total Purchase Value is Rs.{{ totalPurchaseValue }} Only</div>
      </div>
      <br>
      <table class="table table-bordered" v-if="productsView == 'Table'">
        <thead>
        <tr>
          <th scope="col">Sr.No.</th>
          <th scope="col">Title</th>
          <th scope="col">Type</th>
          <th scope="col">Description</th>
          <th scope="col">Price</th>
          <th scope="col">Rating</th>
          <th scope="col">In Stock</th>
          <th scope="col">Add to Cart</th>
          <th scope="col">Remove from Cart</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(product,index) in products" v-bind:key="product.title">
          <td>{{ index + 1 }}</td>
          <td>{{ product.title }}</td>
          <td>{{ product.type }}</td>
          <td>{{ product.description }}</td>
          <td>{{ product.price }}</td>
          <td>{{ product.rating }}</td>
          <td>{{ product.available }}</td>
          <td><button @click="addToCart(product)">Add</button></td>
          <td><button @click="removeFromCart(product)">Remove</button></td>
        </tr>
        </tbody>
      </table>

      <div class="row" v-else>
        <div class="column" style="background-color: lightblue;" v-for="product in products" v-bind:key="product.title">
          <h2>Title: {{ product.title }}</h2>
          <h4>Type: {{ product.type }}</h4>
          <p>{{ product.description }}</p>
          <p>Price: {{ product.price }}</p>
          <p>Rating: {{ product.rating }}</p>
          <p>In Stock: {{ product.available }}</p>
          <p style="margin-top: revert"><button @click="addToCart(product)">Add To Cart</button></p>
          <hr>
          <p><button @click="removeFromCart(product)">Remove from Cart</button></p>
        </div>
      </div>
    </div>
    <div v-else>
      <CompareProducts :products="productsAddedToCart" :productsAddedToCart="productsAddedToCart" @backToProductsView="backToProductsView"></CompareProducts>
    </div>
  </div>
</template>

<script>
import jsonProducts from '../../Assets/ProductDetails.json';
import CompareProducts from "./CompareProducts";
export default {
  name: "Products",
  components: {
    CompareProducts
  },
  data() {
    return {
      products: jsonProducts,
      productsView: 'Table',
      productsAddedToCart: [],
      displayWarning: false,
      productName: '',
      productsAvailable: 0,
      totalPurchaseValue: 0,
      compareProductsView: false
    }
  },
  methods: {
    changeView() {
      if (this.productsView === 'Table') {
        this.productsView = 'Grid'
      }
      else {
        this.productsView = 'Table'
      }
    },
    addToCart(product) {
      this.productName = product.title;
      if ( product.available > 0) {
        this.productsAddedToCart.push(product.title);
        product.available -= 1;
        this.productsAvailable = product.available;
        this.displayWarning = true;
        this.totalPurchaseValue += product.price;
      }
    },
    compareProducts() {
      if (this.productsAddedToCart.length >=1 && this.productsAddedToCart.length <=3) {
        this.compareProductsView = true;
      }
    },
    removeFromCart(product) {
      const index = this.productsAddedToCart.indexOf(product.title);
      if (index > -1) {
        this.productsAddedToCart.splice(index, 1);
        product.available += 1;
        this.productsAvailable = product.available;
        this.displayWarning = true;
        this.totalPurchaseValue -= product.price;
      }
    },
    backToProductsView() {
      this.compareProductsView = false;
    }
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
  margin: 1px;
}

/* Create two equal columns that floats next to each other */
.column {
  float: left;
  width: 50%;
  padding: 10px;
  height: 300px; /* Should be removed. Only for demonstration */
}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

/* Responsive layout - makes the two columns stack on top of each other instead of next to each other */
@media screen and (max-width: 600px) {
  .column {
    width: 100%;
  }
}

.btn {
  border: none;
  outline: none;
  padding: 12px 16px;
  background-color: #f1f1f1;
  cursor: pointer;
}

.compare {
  float: right;
}

.btn:hover {
  background-color: #ddd;
}

.alert {
  padding: 4px;
  background-color: yellow;
  color: black;
}
</style>