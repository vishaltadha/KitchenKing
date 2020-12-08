<template>
  <div>
    <div class="modal fade in modal-active show">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h4 class="modal-title">Ingredients</h4>
            <button type="button" @click="$emit('close')" class="close">
              <span>&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="form-group text-left">
              <label for="">Ingredients</label>
              <div
                class="row m-0"
                v-for="(ing, index) of ingredientsList"
                v-bind:key="index"
              >
                <input
                  class="form-control mr-sm-2 col"
                  v-on:input="handleIngNameChange($event, index)"
                  type="text"
                  v-bind:value="ing.ingName"
                  placeholder="Ing. Name"
                />
                <span @click="removeIngredient(index)">X</span>
              </div>

              <button
                class="btn-md btn-secondary px-3 mt-2"
                @click="addNewIngredient"
              >
                Add Ing.
              </button>
            </div>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-default"
              @click="$emit('close')"
            >
              Close
            </button>
            <button
              type="button"
              class="btn btn-primary"
              @click="saveChanges()"
            >
              Save changes
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "IngredientSelectModal",
  data() {
    return {
      ingredientsList: [{ ingName: "" }],
    };
  },

  created() {
    let ingredients = this.ingredients.split(",");
    this.ingredientsList = ingredients.map((name) => {
      return {
        ingName: name,
      };
    });
  },

  props: {
    ingredients: String,
  },
  components: {},
  methods: {
    addNewIngredient() {
      this.ingredientsList.push({
        ingName: "",
      });
    },
    handleIngNameChange(e, index) {
      this.ingredientsList[index].ingName = e.target.value;
    },
    saveChanges() {
      let ingList = this.ingredientsList.reduce((acc, item) => {
        acc = acc.length > 0 ? acc + "," + item.ingName : item.ingName;
        return acc;
      }, "");
      this.$emit("close", ingList);
    },
    removeIngredient(index) {
      this.ingredientsList.splice(index, 1);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modal-active {
  display: block;
}
</style>
