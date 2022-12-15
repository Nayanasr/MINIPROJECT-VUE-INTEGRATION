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
            <button
              class="btn btn-danger"
              @click="deleteSelectedProd(product.id)"
            >
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

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
        let response = await axios.get("https://localhost:44373/api/Shop/ProductListCart");
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

    // delete the product
    async deleteSelectedProd(ID) {
      try {
        let response = await axios.delete(`https://localhost:44373/api/Shop/DeleteProduct/${ID}`);
        if (response.data.error) {
          let errMsg = response.data.message;
          console.log("errMsg", errMsg);
        } else {
          let resProducts = response.data.listproducts;
          this.products = resProducts;
          console.log("ID", ID);
          this.fetchProducts();
        }
      } catch (err) {
        console.log("err", err);
      }
    },
  },
  mounted() {
    this.fetchProducts();
  },
};
</script>

<style></style>
