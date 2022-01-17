<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />

      <div class="row mt-4">
        <div class="col">
          <h2>Data <strong>Terkini</strong></h2>
        </div>
      </div>

      <div class="row mb-3">
        <div class="col">
          <div>
            <b-form-select
              v-model="selected"
              class="mb-3"
              @change="changeCountry(selected)"
            >
              <b-form-select-option
                v-for="(negara, index) in Negara"
                :key="negara.countries"
                :value="negara.name"
              >
                {{ index + " " + negara.name }}
              </b-form-select-option>
            </b-form-select>
            <div><h5>Terkonfirmasi</h5></div>
            <b-progress
              :value="terkonfirmasi"
              variant="info"
              :max="max"
              striped
              :animated="animate"
            ></b-progress>
            <div>
              <h1>
                <b-badge variant="info">{{
                  terkonfirmasi.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                }}</b-badge>
              </h1>
            </div>
            <div><h5>Sembuh</h5></div>
            <b-progress
              :value="sembuh"
              variant="success"
              :max="max"
              striped
              :animated="animate"
              class="mt-2"
            ></b-progress>
            <div>
              <h1>
                <b-badge variant="success">{{
                  sembuh.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                }}</b-badge>
              </h1>
            </div>
            <div><h5>Meninggal</h5></div>
            <b-progress
              :value="meninggal"
              variant="danger"
              :max="max"
              striped
              :animated="animate"
              class="mt-2"
            ></b-progress>
            <div>
              <h1>
                <b-badge variant="danger">{{
                  meninggal.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
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
import Hero from "@/components/Hero.vue";
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
      Kasus: {},
      terkonfirmasi: 0,
      meninggal: 0,
      sembuh: 0,
      max: 0,
      lastUpdate: "",
      animate: true,
      selected: "Indonesia",
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
    changeCountry(event) {
      axios
        .get("https://covid19.mathdro.id/api/countries/" + event)
        .then((response) => this.setKasus(response.data))
        .catch((error) => console.log(error));
    },
    setKasus(kas) {
      this.Kasus = kas;
      this.terkonfirmasi = kas.confirmed.value;
      this.sembuh = kas.recovered.value;
      this.meninggal = kas.deaths.value;
      this.max = kas.confirmed.value + kas.recovered.value + kas.deaths.value;
      this.lastUpdate = kas.lastUpdate;
    },
  },
  mounted() {
    axios
      .get("https://covid19.mathdro.id/api/countries")
      .then((response) => this.setNegara(response.data))
      .catch((error) => console.log(error));
    axios
      .get("https://covid19.mathdro.id/api/countries/" + this.selected)
      .then((response) => this.setKasus(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
