<template>
  <div v-if="currentItem" class="edit-form">
    <h4>Item</h4>
    <form>
      <div class="form-group">
        <label for="title">Title</label>
        <input type="text" class="form-control" id="title"
          v-model="currentItem.title"
        />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" class="form-control" id="description"
          v-model="currentItem.description"
        />
      </div>

      <div class="form-group">
        <label for="image">Imagen</label>
        <img v-bind:src="currentItem.image" /> 
      </div>

      <div class="form-group">
        <label for="price">price</label>
        <input type="text" class="form-control" id="price"
          v-model="currentItem.price"
        />
      </div>

      <div class="form-group">
        <label><strong>Cantidad:</strong></label>
        <input type="text" class="form-control" id="Cantidad"
          v-model="currentItem.Cantidad"
        />
      </div>
      <div class="form-group">
        <label><strong>Novedad:</strong></label>
        <input type="text" class="form-control" id="Cantidad"
          v-model="currentItem.novedad"
        />
      </div>
    </form>

    <button class="badge badge-primary mr-2"
      v-if="currentItem.published"
      @click="updatePublished(false)"
    >
      UnPublish
    </button>
    <button v-else class="badge badge-primary mr-2"
      @click="updatePublished(true)"
    >
      Publish
    </button>

    <button class="badge badge-danger mr-2"
      @click="deleteItem"
    >
      Delete
    </button>

    <button type="submit" class="badge badge-success"
      @click="updateItem"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Please click on a Item...</p>
  </div>
</template>

<script>
import ItemDataService from "../services/ItemDataService";

export default {
  name: "Item",
  data() {
    return {
      currentItem: null,
      message: ''
    };
  },
  methods: {
    getItem(id) {
      ItemDataService.get(id)
        .then(response => {
          this.currentItem = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updatePublished(reduction) {
      var data = {
        id: this.currentItem.id,
        title: this.currentItem.title,
        description: this.currentItem.description,
        Cantidad: this.currentItem.Cantidad - reduction,
        image: this.currentItem.image,
        novedad: this.currentItem.novedad
      };

      ItemDataService.update(this.currentItem.id, data)
        .then(response => {
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    updateItem() {
      ItemDataService.update(this.currentItem.id, this.currentItem)
        .then(response => {
          console.log(response.data);
          this.message = 'The Item was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteItem() {
      ItemDataService.delete(this.currentItem.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "Items" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getItem(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
