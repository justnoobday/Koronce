<template>
  <div class="food-detail">
    <Navbar />
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6">
          <img :src="'../img/' + product.gambar" class="img-fluid shadow" />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
            <hr />
          </h2>
          <h4>
            Harga :<strong> Rp.{{ product.harga }}</strong>
          </h4>
          <form v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pesanan">Jumlah Pesanan</label>
              <input
                type="number"
                class="form-control"
                v-model="pesan.jumlah_pemesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                v-model="pesan.keterangan"
                class="form-control"
                placeholder="Keterangan Spt Pedas"
              ></textarea>
            </div>

            <button type="submit" class="btn btn-success" @click="pemesanan">
              <b-icon-cart></b-icon-cart>Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";
export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    pemesanan() {
      if (this.pesan.jumlah_pemesanan) {
        this.pesan.product = this.product;
        axios
          .post("http://localhost:3000/keranjangs", this.pesan)
          .then(() => {
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Pesanan Ditambahkan Ke Keranjang", {
              position: "top-right",
            });
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error("Jumlah Pesanan harus diisi!", {
          position: "top-right",
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
