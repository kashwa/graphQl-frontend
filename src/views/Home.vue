<template>
  <div class="home">
    <!-- Apollo Query Categories -->
    <ApolloQuery :query="categoriesQuery">
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <a href="#" @click.prevent="updateSelectedCategory('all')" class="link-margin">All</a>
          <a href="#" @click.prevent="updateSelectedCategory('featured')" class="link-margin">Featured</a>
          <a href="#" v-for="category of data.categories" 
          :key="category.id" class="link-margin"
          @click.prevent="updateSelectedCategory(category.id)">
            {{ category.name }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <!-- Apollo Query Books -->
    <div v-if="selectedCategory === 'all'">
      <ApolloQuery :query="query">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.books" :key="book.id">
              {{ book.id }}) {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>

    <div v-else-if="selectedCategory === 'featured'">
      <ApolloQuery :query="query" :variables="{ featured: true }">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.booksByFeatured" :key="book.id">
              {{ book.id }}) {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>

    <!-- Apollo Query Each Category -->
    <div v-else>
      <ApolloQuery :query="query" :variables="{id: selectedCategory}">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.category.books" :key="book.id">
              {{ book.id }}) {{ book.title }}
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
  </div>
</template>

<script>
import categoryQuery from "@/graphql/queries/Category.gql"
import categoriesQuery from "@/graphql/queries/Categories.gql"
import booksQuery from "@/graphql/queries/Books.gql"
import booksFeaturedQuery from "@/graphql/queries/BooksFeatured.gql"

export default {
  name: "home",
  components: {},
  data() {
    return {
      categoryQuery,
      categoriesQuery,
      booksQuery,
      booksFeaturedQuery,
      selectedCategory: 'all',
      query: booksQuery,
      categories: []
    };
  },
  methods: {
    updateSelectedCategory(category){
      if(category === 'all'){
        this.query = booksQuery
      } else if(category === 'featured'){
        this.query = booksFeaturedQuery
      } else{
        this.query = categoryQuery
      }
      this.selectedCategory = category;
    }
  },
};
</script>

<style scoped>
.link-margin{
  margin-right: 24px;

}
</style>
