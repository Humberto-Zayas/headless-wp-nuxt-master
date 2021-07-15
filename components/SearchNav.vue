<template>
  <div style="width: 100%">
  <b-navbar toggleable="md" type="dark" variant="info" style="width: 100%">
    <b-navbar-brand href="/">Nuxt.js Headless Content</b-navbar-brand>
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <div class="d-flex justify-content-center" style="width: 100%" v-if="!menu">
        <b-spinner variant="primary" type="grow" label="Spinning"></b-spinner>
      </div>
      <b-navbar-nav v-else>
        <b-nav-item v-if="!item.child_items" v-for="item in menu.items" :key="`menu-item-${item.ID}`" href="#">{{item.title}}</b-nav-item>
        <b-nav-item-dropdown v-else :key="`menu-item-${item.ID}`" :text="item.title">
          <b-dropdown-item v-for="childItem in item.child_items" href="#">{{childItem.title}}</b-dropdown-item>
          
        </b-nav-item-dropdown>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        
          <b-form-input v-model="term" @change="searchTerm(term)" size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
        
        

        
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
</template>

<script>
import AppIcon from "@/components/AppIcon.vue";
import axios from 'axios';

export default {
  components: {
    AppIcon
  },
  data() {
    return {
      menu: false,
      term: ''
    }
  },
  beforeMount() {
    this.fetchMenu()
  },
  methods: {
    searchTerm (value) {
      this.$store.commit('updateTerm', value)
    },
    async fetchMenu() {
      axios
        .get(`https://hzayas.us/nuxtwp/wp-json/menus/v1/menus/all-pages`)
        .then(response => {
          // console.log(response.data)
          this.menu = response.data
        })
        .catch(error => console.log("not working"))
    }
  }
}
</script>

<style lang="scss" scoped>
.nav {
  background: #000;
  color: #fff;
  padding: 1em 100px;

  a {
    color: #fff;
    opacity: 0.8;
    &:hover {
      opacity: 1;
    }
  }
}

div {
  align-items: center;
  display: flex;
  height: 100%;
  width: 100%;
}

ul {
  padding-left: 0;

  li {
    display: inline-block;
    margin-right: 30px;
  }
}
</style>
