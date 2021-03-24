<template>
  <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="title">Title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          required
          v-model="item.title"
          name="title"
        />
      </div>
<div class="form-group">
        <label for="id">Id</label>
        <input
          class="form-control"
          id="id"
          required
          v-model="item.id"
          name="id"
        />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input
          class="form-control"
          id="description"
          required
          v-model="item.description"
          name="description"
        />
      </div>
      <div class="form-group">
        <label for="Cantidad">Cantidad</label>
        <input
          class="form-control"
          id="Cantidad"
          required
          v-model="item.Cantidad"
          name="Cantidad"
        />
      </div>
      <div class="form-group">
        <label for="image">image</label>
        <input
          class="form-control"
          id="image"
          required
          v-model="item.image"
          name="image"
        />
      </div>
      <div class="form-group">
        <label for="precio">precio</label>
        <input
          class="form-control"
          id="precio"
          required
          v-model="item.precio"
          name="precio"
        />
      </div>
      <div class="form-group">
        <label for="novedad">novedad</label>
        <input
          class="form-control"
          id="novedad"
          required
          v-model="item.novedad"
          name="novedad"
        />
      </div>
      <button @click="saveItem" class="btn btn-success">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <button class="btn btn-success" @click="newItem">Add</button>
    </div>
  </div>
</template>

<script>
import ItemDataService from "../services/ItemDataService";

export default {
  name: "add-item",
  data() {
    return {
      item: {
        id: null,
        title: "",
        description: "",
        published: false
      },
      submitted: false
    };
  },
  methods: {
    saveItem() {
      var data = {
        id: this.item.id,
        title: this.item.title,
        description: this.item.description,
        Cantidad: this.item.Cantidad,
        image: this.item.image,
        novedad: this.item.novedad,
        precio: this.item.precio
      };

      ItemDataService.create(data)
        .then(response => {
          this.item.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    newItem() {
      this.submitted = false;
      this.item = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>
