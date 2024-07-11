<template>
    <div class="meal-details" v-if="meal">
        <h2>{{ meal.strMeal }}</h2>
        <img :src="meal.strMealThumb" :alt="meal.strMeal" style="max-width: 100%; height: auto;">
        <h3>Ingredients:</h3>
        <ul>
            <li v-for="ingredient in ingredients" :key="ingredient">{{ ingredient }}</li>
        </ul>
        <h3>Instructions:</h3>
        <p>{{ meal.strInstructions }}</p>
    </div>
</template>

<script>
export default {
    props: ['meal'],
    computed: {
        ingredients() {
            const ingredients = [];
            for (let i = 1; i <= 20; i++) {
                if (this.meal[`strIngredient${i}`]) {
                    ingredients.push(`${this.meal[`strIngredient${i}`]} - ${this.meal[`strMeasure${i}`]}`);
                } else {
                    break;
                }
            }
            return ingredients;
        }
    }
}
</script>

<style scoped>
</style>
