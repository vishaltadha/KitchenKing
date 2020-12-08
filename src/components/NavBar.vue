<template>
  <div>
    <nav class="navbar navbar-light bg-light">
      <a class="navbar-brand mr-auto" href="#">Kitchen King</a>
      <div>
        <div class="row">
          <input
            class="form-control mr-sm-2 col"
            type="search"
            v-model="ingredients"
            @click="openIngredientModal"
            placeholder="Ingredients"
          />
          <GoogleLogin
            class="mr-3"
            v-if="!isLoggedIn"
            :params="params"
            :renderParams="renderParams"
            :onSuccess="onSuccess"
            :onFailure="onFailure"
          ></GoogleLogin>
          <div
            v-if="isLoggedIn"
            class="abcRioButton sign-out-btn mr-3 abcRioButtonLightBlue col"
            @click="onGoogleSignOut"
          >
            ({{ userName }}) Sign out
          </div>
        </div>
      </div>
    </nav>
    <IngredientSelectModal
      v-if="showModal"
      :ingredients="ingredients"
      @close="searchRecipe($event)"
    ></IngredientSelectModal>
  </div>
</template>

<script>
import { GoogleLogin, LoaderPlugin } from "vue-google-login";
import IngredientSelectModal from "./IngredientSelectModal";

import Vue from "vue";

Vue.use(LoaderPlugin, {
  client_id:
    "808804559474-nj825rh28bs06glpj7hm0dg5s70s9hqa.apps.googleusercontent.com",
});
export default {
  name: "NavBar",
  data() {
    return {
      // client_id is the only required property but you can add several more params, full list down bellow on the Auth api section
      params: {
        client_id:
          "808804559474-nj825rh28bs06glpj7hm0dg5s70s9hqa.apps.googleusercontent.com",
      },
      showModal: false,
      isLoggedIn: false,
      ingredients: "",
      userName: "",
      // only needed if you want to render the button with the google ui
      renderParams: {
        width: 110,
        height: 36,
        longtitle: false,
      },
    };
  },
  created() {
    Vue.GoogleAuth.then((auth2) => {
      let userInfo = auth2.currentUser.get().wt;
      this.isLoggedIn = !!userInfo;
      this.userName = userInfo && userInfo.Ad;
      console.log(auth2.currentUser.get());
    });
    this.searchRecipe("carrot");
  },
  props: {},
  components: {
    GoogleLogin,
    IngredientSelectModal,
  },
  methods: {
    onSuccess(googleUser) {
      //console.log(googleUser);
      this.isLoggedIn = true;
      let userInfo = googleUser.getBasicProfile();
      // This only gets the user information: id, name, imageUrl and email
      this.userName = userInfo.Ad;
      console.log(googleUser.getBasicProfile());
    },
    onFailure() {},
    onGoogleSignOut() {
      // auth.isSignedIn.get();
      Vue.GoogleAuth.then((auth2) => {
        auth2.signOut().then(() => {
          this.isLoggedIn = false;
        });
      });
    },
    openIngredientModal() {
      if (this.isLoggedIn) {
        this.showModal = true;
      } else {
        alert("Please login with google to search recipe");
      }
    },
    searchRecipe(ingredients) {
      this.showModal = false;
      if (ingredients !== undefined && this.ingredients !== ingredients) {
        this.ingredients = ingredients;
        this.$emit("ingredients", ingredients);
      }
      //   if (ingredients !== undefined && this.ingredients !== ingredients) {
      //     this.ingredients = ingredients;
      //     fetch("http://www.recipepuppy.com/api/?i=" + ingredients)
      //       .then((response) => response.json())
      //       .then((response) => response.results)
      //       .then((data) => {
      //         this.$emit("recipeList", data);
      //       });
      //   }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sign-out-btn {
  height: 36px;
  line-height: 36px;
  font-size: 14px;
}
</style>
