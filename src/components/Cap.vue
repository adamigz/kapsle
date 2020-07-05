<template>
  <div class="m-auto">
    <b-card
      img-src="https://picsum.photos/600/300/?image=25"
      img-alt="Image"
      img-top
      tag="article"
      style="max-width: 20rem;"
      class="mb-2"
    >
      <b-card-title>
        {{item.title}}
      </b-card-title>
      <b-card-text>
        {{item.description}}
      </b-card-text>
      <b-card-text>
        <TheCategory :category="item.category"/>
      </b-card-text>

      <b-button variant="primary" class="m-auto" @click="edit()">Edytuj</b-button>
      &nbsp;
      <b-button variant="danger" class="m-auto" @click="remove()">Usuń</b-button>
    </b-card>
  </div>
</template>
<script>
import TheCategory from './TheCategory.vue'

export default {
  props: {
    item: Object
  },
  components: {
    TheCategory
  },
  methods: {
    edit() {
      let payload = this.item;
      this.$emit('edit', payload);
    },
    remove() {
      localStorage.removeItem('cap'+this.item.index);
      this.$emit('refresh');
    }
  }
};
</script>
