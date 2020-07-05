<template>
  <div class="m-auto">
    <b-card bg-variant="light">
      <b-form-group
        label-cols-lg="3"
        label="Edytuj Kapsel"
        label-size="lg"
        label-class="font-weight-bold pt-0"
        class="mb-0"
      >
        <b-form-group
          label-cols-sm="3"
          label="Tytuł: "
          label-align-sm="right"
          label-for="title"
        >
          <b-form-input id="title" v-model="title"></b-form-input>
        </b-form-group>

        <b-form-group
          label-cols-sm="3"
          label="Opis: "
          label-align-sm="right"
          label-for="description"
        >
          <b-form-input v-model="description" id="description"></b-form-input>
        </b-form-group>

        <b-form-group
          label-cols-sm="3"
          label="Kategoria:"
          label-align-sm="right"
          class="mb-0"
        >
          <b-form-radio-group
            class="pt-2"
            :options="categories"
            v-model="category"
          ></b-form-radio-group>
        </b-form-group>
      </b-form-group>
      <b-button variant="danger" @click="close()" type="button">
        Zamknij <b-icon-plus-circle rotate="45"></b-icon-plus-circle>
      </b-button>
      &nbsp;
      <b-button variant="success" @click="edit()" type="button">
        Edytuj <b-icon-pencil-square></b-icon-pencil-square>
      </b-button>
    </b-card>
    

  </div>
</template>
<script>
export default {
  data() {
    return {
      index: this.item.index,
      title: this.item.title,
      description: this.item.description,
      categories: ["zwykły", "unikalny", "legendarny"],
      category: this.item.category,
      date: null
    };
  },
  computed: {
      fullDate() {
        let day = this.date.getDate();
        if(day<10) {
            day = '0'+day;
        }
        let month = this.date.getMonth()+1;
        if(month<10) {
            month = '0'+month;
        }
        let year = this.date.getFullYear();
        let hour = this.date.getHours();
        if(hour<10) {
            hour = '0'+hour;
        }
        let minute = this.date.getMinutes();
        if(minute<10) {
            minute = '0'+minute;
        }

          return day+'.'+month+'.'+year+' '+hour+':'+minute;
      }
  },
  methods: {
    close() {
      this.$emit("close");
    },
    edit() {
      this.date = new Date;
      //let keyName = localStorage.key(this.index);
      let payload = {
        index: this.index,
        title: this.title,
        description: this.description,
        category: this.category,
        date: this.fullDate,
      };
      localStorage.setItem('cap'+this.index, JSON.stringify(payload));
      this.$emit("edit");
      this.$emit("close");
    },
  },
  props: {
      item: Object
  }
};
</script>
<style scoped>
div {
  margin-bottom: 5px;
}
</style>
