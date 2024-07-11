<template>
    <div class="meal-list">
        <div v-for="meal in meals" :key="meal.idMeal" class="meal-box">
            <img :src="meal.strMealThumb" :alt="meal.strMeal">
            <h3>{{ meal.strMeal }}</h3>
            <button @click="$emit('view-recipe', meal.idMeal)">View Recipe</button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['meals']
}
</script>

<style scoped>

</style>
