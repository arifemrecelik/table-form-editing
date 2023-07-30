<template>
  <div id="app">
    <div
      class="modal fade"
      id="carEditModal"
      tabindex="-1"
      aria-labelledby="carEditModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="carEditModalLabel">
              {{ selectedCar?.cardId }}
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body d-flex flex-column gap-2">
            <div class="d-flex">
              <label class="col-4 text-start">ID</label>
              <span>{{ selectedCar?.id }}</span>
            </div>
            <div class="d-flex">
              <label class="col-4 text-start">Car</label>
              <span>{{ selectedCar?.cardId }}</span>
            </div>
            <div class="d-flex">
              <label class="col-4 text-start">Instock</label>
              <div class="form-check form-check-inline">
                <input
                  v-model="selectedCar.instock"
                  class="form-check-input"
                  type="radio"
                  value="true"
                />
                <label class="form-check-label" for="inlineRadio1">true</label>
              </div>
              <div class="form-check form-check-inline">
                <input
                  v-model="selectedCar.instock"
                  class="form-check-input"
                  type="radio"
                  value="false"
                />
                <label class="form-check-label" for="inlineRadio2">false</label>
              </div>
            </div>
            <div class="d-flex flex-column">
              <div class="d-flex">
                <label class="col-4 text-start">Horse Power</label>
                <input
                  v-model="selectedCar.hp"
                  type="number"
                  min="100"
                  max="550"
                />
              </div>
              <div v-if="hpErrorMessage" class="text-danger d-flex">
                Hp value should be between 100 and 550
              </div>
            </div>
            <div class="d-flex">
              <label class="col-4 text-start">Price</label>
              <input type="number" v-model="selectedCar.price" />
            </div>
            <div class="d-flex">
              <label class="col-4 text-start">Color</label>
              <div>
                <div
                  v-for="(item, index) in colors"
                  :key="index"
                  class="form-check form-check-inline"
                >
                  <input
                    v-model="selectedCar.color"
                    class="form-check-input"
                    type="radio"
                    :value="item.name"
                  />
                  <label class="form-check-label">{{ item.name }}</label>
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <button
              ref="closeButton"
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button
              :disabled="isDisabled"
              type="button"
              class="btn btn-primary"
              @click="save"
            >
              Save
            </button>
          </div>
        </div>
      </div>
    </div>

    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">CARDID</th>
          <th scope="col">INSTOCK</th>
          <th scope="col">HP</th>
          <th scope="col">PRICE</th>
          <th scope="col">COLOR</th>
          <th scope="col">EDIT</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in cars" :key="index">
          <td>{{ item.id }}</td>
          <td>{{ item.cardId }}</td>
          <td>{{ item.instock }}</td>
          <td>{{ item.hp }}</td>
          <td>{{ item.price }}</td>
          <td>
            {{ item.color }}
            <div class="progress" role="progressbar">
              <div
                class="progress-bar"
                :style="`width: 100%; background-color: ${item.color}`"
              ></div>
            </div>
          </td>
          <td>
            <button
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#carEditModal"
              @click="edit(item)"
            >
              Edit
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';
import { isEqual } from 'lodash';

export default {
  name: 'App',
  components: {},
  data() {
    return {
      cars: [],
      colors: [],
      selectedCar: {},
      currentCarData: {},
      isInstock: false,
      isDisabled: true,
      hpErrorMessage: false,
    };
  },
  watch: {
    selectedCar: {
      handler() {
        const { hp } = this.selectedCar;
        this.hpErrorMessage = hp < 100 || hp > 550;
        this.isDisabled =
          isEqual(this.selectedCar, this.currentCarData) || this.hpErrorMessage;
      },
      deep: true,
    },
  },
  mounted() {
    this.getColors();
    this.getCars();
  },
  methods: {
    getColors() {
      axios
        .get('https://64c4dfee67cfdca3b66110f0.mockapi.io/colors')
        .then((response) => {
          this.colors = response.data;
        });
    },
    getCars() {
      axios
        .get('https://64c4dfee67cfdca3b66110f0.mockapi.io/cars')
        .then((response) => {
          this.cars = response.data;
        })
        .catch((error) => {
          console.log('GET CARS - Error Message');
          console.log(error);
        });
    },
    edit(payload) {
      this.selectedCar = { ...payload };
      this.currentCarData = { ...payload };
    },
    save() {
      axios
        .put(
          `https://64c4dfee67cfdca3b66110f0.mockapi.io/cars/${this.selectedCar.id}`,
          this.selectedCar
        )
        .then(() => {
          this.$refs.closeButton.click();
          this.getCars();
        })
        .catch((error) => {
          console.log('SAVE - Error Message');
          console.log(error);
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.table > tbody > tr > td {
  vertical-align: middle;
}
</style>
