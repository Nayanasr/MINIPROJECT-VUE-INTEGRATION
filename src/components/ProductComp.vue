<template>
  <div>
    <div className="container mt-3">
      <div className="row d-flex justify-content-center align-items-center">
        <div
          v-for="product in products"
          :key="product.id"
          class="card"
          style="width: 18rem"
        >
          <img
            class="card-img-top"
            :src="`../../assets/Images/${product.image}`"
            alt="Card image cap"
          />
          <div class="card-body">
            <p class="card-text">{{ product.id }}</p>
            <h5 class="card-title">{{ product.name }}</h5>
            <p class="card-text">Actual Price: {{ product.actualPrice }}</p>
            <p class="card-text">Discount Price: {{ product.discountedPrice }}</p>
            <button class="btn btn-success" @click="addToCart(product)">
              Add to cart
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  components: {},
  data() {
    return {
      products: [],
    };
  },

  methods: {
    //Get products list
    async fetchProducts() {
      try {
        let response = await axios.get("https://localhost:44373/api/Shop/ProductList");
        if (response.data.error) {
          let errMsg = response.data.message;
          console.log("errMsg", errMsg);
        } else {
          let resProducts = response.data.listproducts;
          this.products = resProducts;
          console.log("response.data.listproducts", response.data.listproducts);
          console.log("products", this.products);
        }
      } catch (err) {
        console.log("err", err);
      }
    },

    async addToCart(addProduct) {
      axios.post("https://localhost:44373/api/Shop/AddProduct", addProduct)
        .then((result) => {
          if (result.data.statusCode === "200") {
            alert("item added to Cart");
          }
          console.log("addProduct", addProduct);
          console.log("Added Cart item", result.data);
          this.$router.push("/cart");
        })
        .catch((error) => {
          console.log(error.message);
        });
    },
  },
  mounted() {
    this.fetchProducts();
  },
};
</script>

<style></style>
