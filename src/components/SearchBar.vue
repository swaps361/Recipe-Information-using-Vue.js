<template>
    <div class="search-container">
        <input type="text" v-model="query" @keypress.enter="searchMeal" placeholder="Search for a meal...">
        <button @click="searchMeal">Search</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            query: ''
        }
    },
    methods: {
        searchMeal() {
            this.$emit('search', this.query);
        }
    }
}
</script>

<style scoped>

</style>
