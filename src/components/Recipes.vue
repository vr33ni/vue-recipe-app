<script setup>
import { v4 as uuidv4 } from "uuid";
import { ref, computed } from "vue";
import { Modal } from "bootstrap";
import IngredientsCheckModal from "./Modal.vue";

defineProps({
  msg: String,
});

const nonVeganFoods = ref(["meat", "milk", "eggs", "butter", "sausage"]);
const ingredientsArray = ref([]);
const recipes = ref([]);
const newRecipe = ref({
  number: null,
  color: "",
  name: "",
  ingredients: "",
  steps: "",
});

const isVegan = computed(() => {
  const ingredientsArray = newRecipe.value.ingredients.split(" ");
  const containsNonVeganProducts = ingredientsArray.some((el) =>
    nonVeganFoods.value.includes(el)
  );
  console.log(ingredientsArray, containsNonVeganProducts);

  return !containsNonVeganProducts;
});

const recipesCount = computed(() => {
  return recipes.value.length;
});

function addRecipe() {
  if (!isVegan.value) {
    //open modal asking if recipe contains meat;
    console.log("contains meat?");
    const modalContainer = document.getElementById("veganCheck");
    const modal = new Modal(modalContainer, "show");
    modal.show();
  } else {
    newRecipe.value.number = recipesCount.value + 1;
    newRecipe.value.color =
      "#" + ((Math.random() * 0xffffff) << 0).toString(16);
    recipes.value.push({
      id: uuidv4(),
      ...newRecipe.value,
    });
      resetNewRecipe();

  }
}
function deleteRecipe(index) {
  recipes.value.splice(index, 1);
}

function resetNewRecipe() {
  newRecipe.value.name = "";
  newRecipe.value.ingredients = "";
  newRecipe.value.steps = "";
}
</script>

<template>
  <div class="container">
    <h1>{{ msg }}</h1>

    <form @submit.prevent="addRecipe">
      <div class="row mb-3">
        <label for="name" class="col-sm-2 col-form-label">Name</label>
        <div class="col-sm-10">
          <input
            type="text"
            class="form-control"
            id="name"
            v-model="newRecipe.name"
            required
          />
        </div>
      </div>

      <div class="row mb-3">
        <label for="ingredients" class="col-sm-2 col-form-label"
          >Ingredients</label
        >
        <div class="col-sm-10">
          <input
            type="text"
            class="form-control"
            id="ingredients"
            v-model="newRecipe.ingredients"
            required
          />
        </div>
      </div>
      <div class="row mb-3">
        <label for="steps" class="col-sm-2 col-form-label">Steps</label>
        <div class="col-sm-10">
          <input
            type="text"
            class="form-control"
            id="steps"
            v-model="newRecipe.steps"
            required
          />
        </div>
      </div>
      <button type="submit">Add new recipe</button>
    </form>
    <hr />

    <div v-for="(r, index) of recipes" :key="r.name">
      <div class="card mb-3" v-bind:style="{ backgroundColor: r.color }">
        <div class="row g-0">
          <div class="col-md-4">
            <img src="..." class="img-fluid rounded-start" alt="..." />
          </div>
          <div class="col-md-8">
            <h5
              class="
                card-header
                d-flex
                justify-content-between
                align-items-center
              "
            >
              #{{ r.number }}: {{ r.name }}
              <button
                type="button"
                class="btn btn-sm btn-primary"
                @click="deleteRecipe(index)"
              >
                x
              </button>
            </h5>
            <div class="card-body">
              <h5 class="card-title">Ingredients</h5>
              <p class="card-text">{{ r.ingredients }}</p>
              <h5 class="card-title">Steps</h5>

              <p class="card-text">{{ r.steps }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <IngredientsCheckModal id="veganCheck" :nonVeganFoods="nonVeganFoods" />
  </div>
</template>

