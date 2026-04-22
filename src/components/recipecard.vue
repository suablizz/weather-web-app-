<template>
  <div class="card">
    <img :src="meal.image" alt="meal" />
    <h3>{{ meal.name }}</h3>
    <p><strong>Cuisine:</strong> {{ meal.cuisine }}</p>
    <p><strong>Difficulty:</strong> {{ meal.difficulty }}</p>
    <p><strong>Rating:</strong> ⭐ {{ meal.rating }}</p>
    <p><strong>Cook Time:</strong> {{ meal.cookTime }} mins</p>
  </div>
</template>

<script>
export default {
  props: ["meal"]
};
</script>

<style>
.card {
  border: 1px solid #ddd;
  padding: 15px;
  border-radius: 10px;
  background: white;
  text-align: center;
}

.card img {
  width: 100%;
  border-radius: 10px;
}
</style>