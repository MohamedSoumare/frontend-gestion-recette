<template>
  <div class="p-4 w-50 mx-auto">
    <div class="container">
      <h3><i class="fas fa-plus me-2"></i>{{ $t("addRecipe") }}</h3>
      <form @submit.prevent="onSubmit">
        <!-- Champ titre -->
        <div class="mb-3">
          <label for="title" class="form-label">{{ $t("title") }}</label>
          <input
            type="text"
            v-model="title"
            id="title"
            class="form-control"
            required
          />
        </div>

        <!-- Champ ingrédients -->
        <div class="mb-3">
          <label for="ingredients" class="form-label">{{ $t("ingredients") }}</label>
          <input
            type="text"
            v-model="ingredients"
            id="ingredients"
            class="form-control"
            required
          />
        </div>

        <!-- Sélection du type -->
        <div class="mb-3">
          <label for="type" class="form-label">{{ $t("type") }}</label>
          <select v-model="type" id="type" class="form-select" required>
            <option value="dessert">{{ $t("dessert") }}</option>
            <option value="entree">{{ $t("starter") }}</option>
            <option value="plat">{{ $t("main") }}</option>
          </select>
        </div>

        <!-- Sélection de la catégorie -->
        <div class="mb-3">
          <label for="categorie" class="form-label">{{ $t("category") }}</label>
          <select v-model="categorie" id="categorie" class="form-select" required>
            <option
              v-for="cat in store.categories"
              :key="cat.id"
              :value="cat.id"
            >
              {{ cat.name }}
            </option>
          </select>
        </div>

        <button type="submit" class="btn btn-success">
          {{ $t("submit") }}
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRecetteStore } from "../../store/recetteStore";
import { useRouter } from "vue-router";

const store = useRecetteStore();
const router = useRouter();

const title = ref("");
const ingredients = ref(""); 
const type = ref("");
const categorie = ref("");

const onSubmit = async () => {
  await store.add({
    title: title.value,
    ingredients: ingredients.value, 
    type: type.value,
    categorie: categorie.value,
  });
  router.push("/recette-list");
};

// Chargement des catégories et des recettes au montage du composant
onMounted(async () => {
  await store.fetchCategories();
  await store.fetchRecettes();
});
</script>

<style scoped>
/* Styles personnalisés peuvent être ajoutés ici */
</style>
