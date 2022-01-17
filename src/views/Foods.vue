<template>
  <div class="Foods">
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>List <strong>Makanan</strong></h2>
        </div>
      </div>
      <div class="input-group mb-3">
        <input
          v-model="search"
          type="text"
          class="form-control"
          placeholder="Cari Makanan"
          aria-label="Username"
          aria-describedby="basic-addon1"
          @keyup="searchFood"
        />
        <span class="input-group-text" id="basic-addon1"
          ><b-icon-search></b-icon-search
        ></span>
      </div>

      <div class="row mb-3">
        <div
          class="col-md-4 mt-4 mb-5"
          v-for="product in products"
          :key="product.id"
        >
          <card-product :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";
export default {
  name: "Foods",
  components: {
    Navbar,
    CardProduct,
  },

  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    searchFood() {
      axios
        .get("http://localhost:3000/products?q=" + this.search)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products")
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style></style>
