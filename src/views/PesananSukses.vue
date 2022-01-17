<template>
  <div class="SuksesPesan">
    <Navbar />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <img src="../assets/menunggu.png" alt="" />
          <h2><strong>Yeay! Berhasil.</strong></h2>
          <h4>Pesanan anda berhasil diproses, selamat menunggu</h4>
          <h4>Mau Pesan Lagi ?</h4>
          <router-link to="/" class="btn btn-light">Pesan Disini</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
export default {
  name: "PesananSukses",
  components: {
    Navbar,
  },
};
</script>

<style></style>
