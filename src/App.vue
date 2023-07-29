<template>
  <div id="app">
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Modal title</h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">...</div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>

    <table class="table">
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
        <tr>
          <td>1</td>
          <td>HJKFSHJK42525fsdfs</td>
          <td>True</td>
          <td>250</td>
          <td>50.000,00 €</td>
          <td>Red</td>
          <td>
            <button
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              @click="edit"
            >
              Edit
            </button>
          </td>
        </tr>
        <tr>
          <td>2</td>
          <td>HFJKAHAK6872431</td>
          <td>False</td>
          <td>140</td>
          <td>30.000,00 €</td>
          <td>Blue</td>
          <td>
            <button
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              @click="edit"
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
export default {
  name: 'App',
  components: {},
  methods: {
    edit() {
      console.log('Edit');
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
</style>
