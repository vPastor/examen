<template>
<div>
<h4>Carro</h4>
<table class="list-group" v-for="(item, index) in items" :key="index">
        <th class="list-group-item" :class="{ active: index == currentIndex }"> 
       <p>
            {{ item.title }}</p>
            <img v-bind:src="item.image" />
            <p>{{ item.price}}</p>
            <p>Cantidad Maxima: {{item.Cantidad}}</p>
            <input :max="item.cantidad" type="number">
             <button class="btn btn-success" @click="removeItem(item.id)">Remove Item</button>
          </th>
          </table>
          <button @click="console.log('YA HAS HECHO TU COMPRA!')" >COMPRAR</button>
</div>
  

</template>

<script>
import ItemDataService from "../services/ItemDataService";

export default {
  name: "Item",
  data() {
   return {
      items: [],
      currentItem: null,
      currentIndex: -1,
      title: ""
    };
  },
  methods: {
    retrieveItems() {
      ItemDataService.getcarrito()
        .then(response => {
          this.items = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveItems();
      this.currentItem = null;
      this.currentIndex = -1;
    },

    removeItem(id) {
      ItemDataService.removeFromCart(id);
      this.refreshList();
      ItemDataService.get(id)
        .then(response => {
          this.currentItem = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    setActiveItem(item, index) {
      this.currentItem = item;
      this.currentIndex = index;
    },

    removeAllItems() {
      ItemDataService.deleteAll()
        .then(response => {
          console.log(response.data);
          this.refreshList();
        })
        .catch(e => {
          console.log(e);
        });
    },
    comprar(item){
      ItemDataService.añadirCarrito(item)
      .then(response => {
          this.items = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    }

    },
    searchTitle() {
      ItemDataService.findByTitle(this.title)
        .then(response => {
          this.items = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    
  },
  mounted() {
    this.retrieveItems();
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
