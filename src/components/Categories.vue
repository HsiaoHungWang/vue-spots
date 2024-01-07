<template>
    <ul v-if="categories && categories.length > 0" class="list-group list-group-horizontal">
        <li :class="{ 'current': category.categoryId == theCategoryId, 'list-group-item': true }"
            v-for="category in categories" :key="category.categoryId" @click="clickHandler(category.categoryId)">
            <button type="button" class="btn">{{ category.categoryName }}</button>
        </li>
    </ul>
</template>
    
<script setup>
import { onMounted, reactive, ref } from "vue";
import axios from 'axios'
const categories = reactive([])
const theCategoryId = ref(0)

const emit = defineEmits(["categoryClick"])
const clickHandler = id => {
    theCategoryId.value = id
    emit("categoryClick", id)
}


const loadCategories = async () => {
    const API_URL = `${import.meta.env.VITE_API_SPOTURL}/categories`
    const response = await axios.get(API_URL)
    categories.push(...response.data)
}

onMounted(async () => {
    loadCategories();

})
</script>
    
<style scoped>
.current {
    background-color: lightblue;
}
</style>