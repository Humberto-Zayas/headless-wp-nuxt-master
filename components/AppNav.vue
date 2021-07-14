<template>
  <div style="width: 100%">
  <b-navbar toggleable="md" type="dark" variant="info" style="width: 100%">
    <b-navbar-brand href="#">Vue Nuxt Headless</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item v-if="!item.child_items" v-for="item in menu.items" :key="`menu-item-${item.ID}`" href="#">{{item.title}}</b-nav-item>
        <b-nav-item-dropdown v-else :key="`menu-item-${item.ID}`" :text="item.title">
          <b-dropdown-item v-for="childItem in item.child_items" href="#">{{childItem.title}}</b-dropdown-item>
          
        </b-nav-item-dropdown>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
        </b-nav-form>

        

        
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
    }
  },
  beforeMount() {
    this.fetchMenu()
  },
  methods: {
    async fetchMenu() {
      axios
        .get(`http://localhost/nuxt/wp-json/menus/v1/menus/all-pages`)
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
