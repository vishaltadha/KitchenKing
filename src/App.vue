<template>
  <div id="app">
    <NavBar @ingredients="renderRecipes($event)" />
    <h5 class="text-left mt-2 ml-3">Recipes</h5>
    <div class="row m-0 mt-3">
      <div
        class="col-md-2 col-sm-3 font-sm"
        v-for="(recipe, index) of recipes"
        v-bind:key="index"
      >
        <div class="card mt-2">
          <img v-bind:src="recipe.thumbnail" alt="" />
          <p class="recipe-name p-2">
            <a v-bind:href="recipe.href" target="_blank">{{ recipe.title }}</a>
          </p>
        </div>
      </div>
    </div>
    <PaginationComponent
      containerWidth="100%"
      v-bind:pageSize="pageSize"
      v-bind:totalRecords="totalRecords"
      v-bind:currentPage="currentPage"
      @gotopage="handlePageChange($event)"
    />
    <p class="font-sm text-right mr-3 mt-2" v-if="isLoading">Loading...</p>
  </div>
</template>

<script>
import NavBar from "./components/NavBar.vue";
import PaginationComponent from "./components/PaginationComponent";
export default {
  name: "App",
  components: {
    NavBar,
    PaginationComponent,
  },
  data() {
    return {
      recipes: [],
      ingredients: "",
      pageSize: 10,
      totalRecords: 520,
      currentPage: 1,
      isLoading: false,
    };
  },
  methods: {
    renderRecipes(ingredients) {
      this.ingredients = ingredients;
      if (ingredients !== undefined) {
        this.fetchRecipes(ingredients, 1);
      }

      //alert(JSON.toString(recipes));
    },

    fetchRecipes(ingredients, pageNo) {
      this.isLoading = true;
      fetch("http://www.recipepuppy.com/api/?i=" + ingredients + "&p=" + pageNo)
        .then((response) => response.json())
        .then((response) => response.results)
        .then((data) => {
          this.recipes = data;
          this.isLoading = false;
        });
    },

    handlePageChange(pageNo) {
      this.fetchRecipes(this.ingredients, pageNo);
      this.currentPage = pageNo;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.font-sm {
  font-size: 14px;
}

.recipe-name {
  height: 40px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
