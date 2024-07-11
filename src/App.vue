<template>
    <div class="container">
        <h1 class="red-heading">Food Recipe</h1>
        <SearchBar @search="searchMeal" />
        <MealList :meals="meals" @view-recipe="viewRecipe" />
        <div ref="mealDetails">
            <MealDetails :meal="selectedMeal" />
        </div>
        <MealList :meals="recommendedMeals" v-if="recommendedMeals.length" />
    </div>
</template>

<script>
import SearchBar from './components/SearchBar.vue';
import MealList from './components/MealList.vue';
import MealDetails from './components/MealDetails.vue';

export default {
    components: {
        SearchBar,
        MealList,
        MealDetails
    },
    data() {
        return {
            meals: [],
            selectedMeal: null,
            recommendedMeals: []
        }
    },
    methods: {
        displayDefaultMeals() {
            const defaultMealURLs = [
                'https://www.themealdb.com/api/json/v1/1/search.php?s=Arrabiata',
                'https://www.themealdb.com/api/json/v1/1/search.php?s=Chicken',
                'https://www.themealdb.com/api/json/v1/1/search.php?s=Salmon',
                'https://www.themealdb.com/api/json/v1/1/search.php?s=Salad',
                'https://www.themealdb.com/api/json/v1/1/search.php?s=Seafood',
                'https://www.themealdb.com/api/json/v1/1/search.php?s=Vegetarian',
            ];

            defaultMealURLs.forEach(url => {
                fetch(url)
                    .then(response => response.json())
                    .then(data => {
                        if (data.meals) {
                            this.meals.push(data.meals[0]);
                        }
                    })
                    .catch(error => console.error('Error fetching the meal data:', error));
            });
        },
        searchMeal(query) {
            const apiURL = `https://www.themealdb.com/api/json/v1/1/search.php?s=${query}`;

            fetch(apiURL)
                .then(response => response.json())
                .then(data => {
                    if (data.meals) {
                        this.selectedMeal = data.meals[0];
                        this.recommendedMeals = [];

                        for (let i = 0; i < 3; i++) {
                            fetch('https://www.themealdb.com/api/json/v1/1/random.php')
                                .then(response => response.json())
                                .then(data => {
                                    if (data.meals) {
                                        this.recommendedMeals.push(data.meals[0]);
                                    }
                                })
                                .catch(error => console.error('Error fetching the recommended meals:', error));
                        }

                        // Scroll to meal details
                        this.$nextTick(() => {
                            this.$refs.mealDetails.scrollIntoView({ behavior: 'smooth' });
                        });
                    } else {
                        this.selectedMeal = null;
                    }
                })
                .catch(error => console.error('Error fetching the meal data:', error));
        },
        viewRecipe(mealID) {
            const apiURL = `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${mealID}`;

            fetch(apiURL)
                .then(response => response.json())
                .then(data => {
                    if (data.meals) {
                        this.selectedMeal = data.meals[0];
                        // Scroll to meal details
                        this.$nextTick(() => {
                            this.$refs.mealDetails.scrollIntoView({ behavior: 'smooth' });
                        });
                    } else {
                        this.selectedMeal = null;
                    }
                })
                .catch(error => console.error('Error fetching the recipe:', error));
        }
    },
    mounted() {
        this.displayDefaultMeals();
    }
}
</script>

<style>
/* Your styles from the original CSS */
body {
    font-family: Arial, sans-serif;
    background-image: url('./assets/food.jpg'); /* Updated path */
    background-size: cover;
    background-position: center;
    margin: 0;
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

.container {
    text-align: center;
    max-width: 800px;
    width: 100%;
    background: rgba(255, 255, 255, 0.9);
    padding: 20px;
    border-radius: 8px;
}

h1 {
    color: red; 
}

.search-container {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
}

input[type="text"] {
    padding: 10px;
    width: 70%;
    margin-right: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

button {
    padding: 10px 20px;
    background-color: #000; 
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

button:hover {
    background-color: #333; 
}

.meal-list {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    margin-bottom: 20px;
}

.meal-box {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    text-align: left;
}

.meal-box img {
    width: 100%;
    border-radius: 8px;
}

.meal-box button {
    margin-top: 10px;
    padding: 10px 20px;
    background-color: #a17715;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    width: 100%;
}

.meal-box button:hover {
    background-color: #218838;
}

.meal-details {
    margin-top: 20px;
    text-align: left;
}

.meal-details h2 {
    margin-bottom: 10px;
}

.meal-details ul {
    list-style: none;
    padding: 0;
}

.meal-details ul li {
    margin-bottom: 5px;
}

.hidden {
    display: none;
}
</style>
