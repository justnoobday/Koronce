<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />

      <div class="row mt-4">
        <div class="col">
          <h2>Data <strong>Terkini Vaksinasi Covid-19</strong></h2>
          <hr />
        </div>
      </div>

      <div class="row mb-3">
        <div class="col">
          <div>
            <div><h5>Total Sasaran Vaksinasi</h5></div>
            <div>
              <h1>
                <b-badge variant="info">{{
                  totalSasaranVaksin
                    .toString()
                    .replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                }}</b-badge>
              </h1>
            </div>
            <div><h5>Sasaran Vaksinasi SDMK</h5></div>
            <div>
              <h1>
                <b-badge variant="primary">{{
                  vaksinSdmk.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                }}</b-badge>
              </h1>
            </div>
            <div><h5>Sasaran Vaksinasi Lansia</h5></div>
            <div>
              <h1>
                <b-badge variant="info">{{
                  vaksinLansia.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                }}</b-badge>
              </h1>
            </div>
            <div><h5>Sasaran Vaksinasi Petugas Publik</h5></div>
            <div>
              <h1>
                <b-badge variant="info">{{
                  vaksinPelayanan
                    .toString()
                    .replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                }}</b-badge>
              </h1>
            </div>
            <div><h5>Vaksinasi Dosis 1</h5></div>
            <b-progress
              :value="dosis1"
              variant="success"
              :max="max"
              striped
              :animated="animate"
              class="mt-2"
            ></b-progress>
            <div>
              <h1>
                <b-badge variant="success">{{
                  dosis1.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                }}</b-badge>
              </h1>
            </div>
            <div><h5>Vaksinasi Dosis 2</h5></div>
            <b-progress
              :value="dosis2"
              variant="danger"
              :max="max"
              striped
              :animated="animate"
              class="mt-2"
            ></b-progress>
            <div>
              <h1>
                <b-badge variant="danger">{{
                  dosis2.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                }}</b-badge>
              </h1>
            </div>
            <div><h5>Persentase Progres Vaksinasi Total</h5></div>
            <b-progress
              :value="dosis1 + dosis2"
              variant="warning"
              :max="max"
              striped
              :animated="animate"
              class="mt-2"
            ></b-progress>
            <div>
              <h1>
                <b-badge variant="warning">{{
                  parseFloat(persenProgress).toFixed(2) + "%"
                }}</b-badge>
              </h1>
            </div>
          </div>
          <div class="">
            <h1>
              <b-badge variant="light"
                >Last Update Data : {{ lastUpdate.slice(0, 10) }}</b-badge
              >
              <b-badge variant="light"
                >Jam: {{ lastUpdate.slice(11, 19) }}</b-badge
              >
            </h1>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Hero from "@/components/HeroVaksin.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Navbar,
    Hero,
  },
  data() {
    return {
      products: [],
      Negara: {},
      totalSasaranVaksin: 0,
      vaksinSdmk: 0,
      vaksinLansia: 0,
      vaksinPelayanan: 0,
      dosis1: 0,
      dosis2: 0,
      persenProgress: 0,
      max: 0,
      lastUpdate: "",
      animate: true,
      selected: null,
      options: [
        { value: null, text: "Please select an option" },
        { value: "a", text: "This is First option" },
      ],
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    setNegara(neg) {
      this.Negara = neg.countries;
    },
    Vaksin(vak) {
      this.totalSasaranVaksin = vak.totalsasaran;
      this.vaksinSdmk = vak.sasaranvaksinsdmk;
      this.vaksinLansia = vak.sasaranvaksinlansia;
      this.vaksinPelayanan = vak.sasaranvaksinpetugaspublik;
      this.dosis1 = vak.vaksinasi1;
      this.dosis2 = vak.vaksinasi2;
      this.max = vak.totalsasaran;
      this.lastUpdate = vak.lastUpdate;
      this.persenProgress =
        ((vak.vaksinasi1 + vak.vaksinasi2) / vak.totalsasaran) * 100;
    },
  },
  mounted() {
    axios
      .get("http://vaksincovid19-api.now.sh/api/vaksin")
      .then((response) => this.Vaksin(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
