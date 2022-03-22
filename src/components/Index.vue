<template>
  <div class="geolocation">
    <div class="form-group">
      <label for="cep">CEP:</label>
      <input type="text" v-model="cep" class="form-control input-cep" />
    </div>
    <button
      class="btn btn-primary btn-buscar"
      @click="buscarGeolocationPorCep()"
    >
      Buscar
    </button>
    <br />
    <span v-if="latitude != null">Latitude: {{ latitude }}</span>
    <span v-if="longitude != null">Longitude: {{ longitude }}</span>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      latitude: null,
      longitude: null,
      cep: "",
      apiKey: "AIzaSyBLQWRPQ_MlohawM00zz0gSH9IhSvQER4c",
    };
  },
  methods: {
    buscarGeolocationPorCep() {
      if (this.cep.trim() != "") {
        axios
          .get(
            `https://maps.googleapis.com/maps/api/geocode/json?address=${this.cep}&key=${this.apiKey}`
          )
          .then((response) => {
            if (
              response.data.status == "OK" &&
              response.data.results.length > 0
            ) {
              var location = response.data.results[0].geometry.location;
              this.latitude = location.lat;
              this.longitude = location.lng;
            }
            this.cep = "";
          })
          .catch((err) => {
            console.error(err);
          });
      }
    },
  },
};
</script>

<style scoped>
.geolocation {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100vw;
  height: 100vh;
  margin-top: 60px;
}
.btn-buscar {
  margin-top: 10px;
  width: 250px;
}
.input-cep {
  width: 250px;
}
</style>
