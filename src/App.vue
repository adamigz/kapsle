<template>
  <div id="app">
    <b-navbar type="light" tag="nav" variant="light">
      <transition name="fade" mode="out-in">
        <b-button
          v-if="addComponent == false"
          variant="outline-primary"
          @click="showAddComponent()"
          type="button"
        >
          Dodaj<b-icon-plus></b-icon-plus>
        </b-button>
      </transition>
      <b-nav-form class="ml-auto">
        <b-form-input placeholder="Tytuł"></b-form-input>
        <b-button variant="outline-primary" type="button">
          <b-icon-search></b-icon-search>
        </b-button>
      </b-nav-form>
    </b-navbar>
    <b-container fluid>
      <transition name="bounce">
        <b-row v-if="addComponent == true">
          <CapAdd @close="showAddComponent()" @add="getAllItems()" />
        </b-row>
      </transition>
      <transition name="bounce">
        <b-row v-if="editComponent == true">
          <CapEdit @close="showEditComponent()" :item="edditableItem" @edit="getAllItems()" />
        </b-row>
      </transition>
      <b-container fluid v-for="array in items">
        <b-row>
            <Cap :item="item" class="m-auto" @refresh="getAllItems()" @edit="showEditComponent" v-for="item in array"/>
        </b-row>
      </b-container>
    </b-container>
  </div>
</template>

<script>
import Cap from "./components/Cap.vue";
import CapAdd from "./components/CapAdd.vue";
import CapEdit from "./components/CapEdit.vue";

export default {
  name: "App",
  components: {
    Cap,
    CapAdd,
    CapEdit
  },
  data() {
    return {
      addComponent: false,
      editComponent: false,
      items: [],
      edditableItem: {}
    };
  },
  methods: {
    showAddComponent() {
      return (this.addComponent = !this.addComponent);
    },
    showEditComponent(perk) {
      this.edditableItem = perk
      return (this.editComponent = !this.editComponent);
    },
    getAllItems() {
      this.items = [];
      for (var i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) != "loglevel:webpack-dev-server") {
          this.items.push(
            JSON.parse(localStorage.getItem(localStorage.key(i)))
          );
        }
      }
      this.items = this.chunk(this.items, 4);
    },
    chunk(array, size) {
      const chunked_arr = [];
      let copied = [...array]; // ES6 destructuring
      const numOfChild = Math.ceil(copied.length / size); // Round up to the nearest integer
      for (let i = 0; i < numOfChild; i++) {
        chunked_arr.push(copied.splice(0, size));
      }
      return chunked_arr;
    },
  },
  created() {
    this.getAllItems();
  },
};
</script>

<style scoped>
nav {
  margin-bottom: 5px;
}
.bounce-enter-active {
  animation: bounce-in 0.8s;
}
.bounce-leave-active {
  animation: bounce-in 0.8s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.25);
  }
  100% {
    transform: scale(1);
  }
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.8s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

</style>
