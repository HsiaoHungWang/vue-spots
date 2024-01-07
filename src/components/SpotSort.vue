<template>
    <select class="form-select" @change="changeHandler" v-model="theSort">
        <option value="spotId">景點編號升冪</option>
        <option value="-spotId">景點編號降冪</option>
        <option value="spotTitle">景點名稱升冪</option>
        <option value="-spotTitle">景點名稱降冪</option>
    </select>
</template>
    
<script setup>
import { ref } from 'vue'
const theSort = ref("spotId")
const sortItem = ref({
    "dir": false,
    "sort": "spotId"
})
const emit = defineEmits(["sortChange"])
const changeHandler = () => {
    sortItem.value.dir = theSort.value.startsWith('-');
    sortItem.value.sort = theSort.value.replace(/^-/, '');
    emit("sortChange", sortItem.value)
}

</script>
    
<style></style>