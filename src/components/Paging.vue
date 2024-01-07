<template>
    <nav class="mt-3">
        <ul class="pagination">
            <li class="page-item" v-for="(value, index) in totalPages" :key="index" @click="clickHandler(value)">
                <a :class="{ 'currentPage': thePage === value, 'page-link': true }">{{ value }}</a>
            </li>
        </ul>
    </nav>
</template>
    
<script setup>
const props = defineProps({
    totalPages: Number,
    thePage: Number
})
const emit = defineEmits(["childClick"])
const clickHandler = page => {
    emit("childClick", page)
}


</script>
    
<style scoped>
.pagination li {
    cursor: pointer;
}

.currentPage {
    background-color: lightblue;
}
</style>