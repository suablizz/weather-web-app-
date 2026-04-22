<template>
  <div id="app">
    <header class="header">
      <div class="header-content">
        <h1>Chefbox Recipe Discovery</h1>
        <p>Find your next favorite recipe from amazing home cooks!</p>
        <!-- Search Bar -->
        <div class="search-box">
          <input 
            v-model="searchText" 
            @input="searchRecipes" 
            type="text" 
            placeholder="Search by name or cuisine..."
            class="search-input"
          >
          <button @click="searchRecipes">🔍 Search</button>
        </div>
      </div>
    </header>

    <div class="recipes-container">
      <div v-if="isLoading" class="loading">Loading recipes...</div>
      <div v-else-if="!recipes.length" class="no-data">No recipes available</div>
      <div v-else class="recipes-list">
        <div 
          v-for="recipe in filteredRecipes" 
          :key="recipe.id"
          class="recipe-card-wrapper"
        >
          <RecipeCard :meal="recipe" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RecipeCard from './components/RecipeCard.vue'

export default {
  name: 'App',
  components: {
    RecipeCard
  },
  data() {
    return {
      recipes: [],
      filteredRecipes: [],
      searchText: '',
      isLoading: true
    }
  },
  async mounted() {
    await this.getRecipes()
  },
  methods: {
    async getRecipes() {
      try {
        this.isLoading = true
        const response = await fetch('https://levelthreesodsecondtermexam.vercel.app/recipes')
        if (!response.ok) throw new Error('Failed to fetch')
        this.recipes = await response.json()
        this.filteredRecipes = [...this.recipes]
      } catch (err) {
        console.error('Fetch error:', err)
        alert('Could not load recipes :(')
      } finally {
        this.isLoading = false
      }
    },
    searchRecipes() {
      const query = this.searchText.toLowerCase().trim()
      if (!query) {
        this.filteredRecipes = [...this.recipes]
        return
      }
      this.filteredRecipes = this.recipes.filter(recipe => 
        recipe.name.toLowerCase().includes(query) ||
        recipe.cuisine.toLowerCase().includes(query)
      )
    }
  }
}
</script>

<style scoped>
#app {
  min-height: 100vh;
}

.header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem 1rem;
  text-align: center;
}

.header-content {
  max-width: 800px;
  margin: 0 auto;
}

.header h1 {
  font-size: 2.5rem;
  margin: 0 0 0.5rem 0;
  font-weight: bold;
}

.header p {
  font-size: 1.1rem;
  margin: 0 0 1.5rem 0;
  opacity: 0.9;
}

.search-box {
  display: flex;
  max-width: 400px;
  margin: 0 auto;
  gap: 0.5rem;
}

.search-input {
  flex: 1;
  padding: 0.8rem 1rem;
  border: none;
  border-radius: 25px;
  font-size: 1rem;
  outline: none;
}

.search-box button {
  padding: 0.8rem 1.5rem;
  background: rgba(255,255,255,0.2);
  color: white;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  font-weight: 500;
}

.search-box button:hover {
  background: rgba(255,255,255,0.3);
}

.recipes-container {
  padding: 2rem 1rem;
  max-width: 1400px;
  margin: 0 auto;
}

.loading, .no-data {
  text-align: center;
  padding: 3rem;
  font-size: 1.2rem;
  color: #666;
}

.recipes-list {
  display: grid;
  gap: 1.5rem;
}



/* Responsive breakpoints */
@media (max-width: 576px) {
  .recipes-list {
    grid-template-columns: 1fr;
  }
}

@media (min-width: 577px) and (max-width: 992px) {
  .recipes-list {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 993px) and (max-width: 1200px) {
  .recipes-list {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (min-width: 1201px) {
  .recipes-list {
    grid-template-columns: repeat(4, 1fr);
  }
}
</style>

