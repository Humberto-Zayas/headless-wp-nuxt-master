<template>
  <div>
    <search-masthead :inputData.sync="parentData"></search-masthead>
    <div class="posts">
      <main>
        <div v-if="!parentData == ''">
            <div class="post"  v-for="post in pageOfItems" :key="post.id">
              <h3>
                <a :href="`blog/${post.slug}`">{{ post.title.rendered }}</a>
              </h3>
              <small>{{ post.date | dateformat }}</small>
              <div v-html="post.excerpt.rendered"></div>
              <a :href="`blog/${post.slug}`" class="readmore slide">Read more ⟶</a>
            </div>
            <div class="card-footer pb-3 pt-3">
                <jw-pagination :items="filteredList" :pageSize="3" @changePage="onChangePage"></jw-pagination>
            </div>
        </div>
        <div v-else>
            <b-alert show variant="dark">
                No results found. Please search again.
            </b-alert>
        </div>
        
      </main>
      <!--  -->
    </div>
  </div>
</template>

<script>
import AppMasthead from "@/components/AppMasthead.vue";
import SearchMasthead from '~/components/SearchMasthead.vue';




export default {
  layout: 'searchlayout',
  components: {
    AppMasthead,
    SearchMasthead
  },
  data() {
    return {
      selectedTag: null,
      activeClass: "active",
      parentData: "",
      pageSize: 3,
      pageOfItems: []
    };
  },
  computed: {
    posts() {
      return this.$store.state.posts;
    },
    tags() {
      return this.$store.state.tags;
    },
    sortedPosts() {
      if (!this.selectedTag) return this.posts;
      return this.posts.filter(el => el.tags.includes(this.selectedTag));
    },
    filteredList() {
      return this.posts.filter(post => {
        return post.title.rendered.toLowerCase().includes(this.parentData.toLowerCase())
      })
    }
  },
  created() {
    this.$store.dispatch("getPosts");
    this.queryCheck()
  },
  methods: {
    queryCheck() {
        if (this.$route.query) {
            this.parentData = this.$route.query.query
        }
    },
    updateTag(tag) {
      if (!this.selectedTag) {
        this.selectedTag = tag.id;
      } else {
        this.selectedTag = null;
      }
    },
    onChangePage(pageOfItems) {
            // update page of items
        this.pageOfItems = pageOfItems;
    }
  }
};
</script>

<style lang="scss">

.pagination {
    .active {
     border: none !important;
    }
}
.posts {
  display: grid;
  grid-template-columns: 2fr 1fr;
  grid-template-rows: 1fr;
  grid-column-gap: 6vw;
  margin: 5em auto;
  max-width: 80vw;
}

main {
  grid-area: 1 / 1 / 2 / 2;
}

aside {
  grid-area: 1 / 2 / 2 / 3;
}

h2 {
  margin-bottom: 2em;
}

a,
a:active,
a:visited {
  text-decoration: none;
  color: black;
}

a.readmore {
  display: inline-block;
  font-size: 11px;
  text-transform: uppercase;
  padding: 5px 15px;
  letter-spacing: 2px;
  position: relative;
  color: #000;
  font-weight: 700;
  font-family: "Open Sans", serif;
  border: 1px solid #ccc;
  background: #fff;
}

.tags-title {
  background-color: #000;
  color: #fff;
  border: none;
  text-transform: capitalize;
  letter-spacing: 0;
  font-size: 1.2rem;
  padding: 15px;
  margin: 0 35px;
  position: relative;
  top: -25px;
}

.tags-list {
  background: #f5f5f5;
  padding: 70px 25px 25px;
  margin-top: -65px;
}

.post {
  border-bottom: 1px solid rgb(223, 222, 222);
  margin-bottom: 2em;
  padding-bottom: 2em;
  color: #444;

  h3 {
    margin-bottom: 0.5em;
    font-size: 26px;
  }
}

.tags-list ul {
  padding-left: 0;
}

.tags-list li {
  font-family: "Open Sans", serif;
  letter-spacing: 1px;
  text-transform: uppercase;
  padding: 6px 15px;
  margin: 0 0 10px 10px;
  display: inline-block;
  font-size: 0.7rem !important;
  border: 1px solid #000;
  transition: all 0.3s;
  outline: none;
  font-weight: normal;
  cursor: pointer;
  background: #fff;
  a {
    color: #000;
  }
}

.active {
  border: 1px solid #d44119;
  background-color: #fae091 !important;
}

.slide {
  position: relative;
  background: transparent;
  -webkit-transition: 0.3s ease;
  transition: 0.3s ease;
  z-index: 1;
  backface-visibility: hidden;
  perspective: 1000px;
  transform: translateZ(0);
  cursor: pointer;

  &:hover {
    color: #fff;
  }

  &:hover:before {
    right: -1px;
  }
}

.slide::before {
  content: "";
  display: block;
  position: absolute;
  background: #000;
  transition: right 0.3s ease;
  z-index: -1;
  top: -2px;
  bottom: -2px;
  left: -2px;
  right: 108%;
  backface-visibility: hidden;
}
</style>
