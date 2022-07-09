<template>
  <div
    class="modal fade"
    id="veganCheck"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">
            Are you sure, this is a vegan recipe?
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          Your ingredients list contains at least one of the following products:
          <ul v-for="(ingredient, index) of nonVeganFoods" :key="index">
            <li>{{ ingredient }}</li>
          </ul>
          Please try again.
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Close
          </button>
          <!-- <button type="button" class="btn btn-primary">Yes</button> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, toRefs } from "vue";

const props = defineProps({
  id: String,
  nonVeganFoods: []
});

const { id, nonVeganFoods } = toRefs(props);
console.log("non vegan foods ", nonVeganFoods.value)
 
</script>