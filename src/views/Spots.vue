<template>
    <h2>台北市景點資料</h2>

    <div class="row">
        <div class="col-3">
            <PageSize @PageSizeChange="changeHandler"></PageSize>

        </div>
        <div class="col-3">

        </div>
        <div class="col-3">
            <SpotSort @sortChange="sortChangeHandler"></SpotSort>
        </div>
        <div class="col-3">
            <SearchTextBox @searchInput="inputHandler"></SearchTextBox>
        </div>
        <div class="col-12">
            <Categories @categoryClick="categoryClickHandler"></Categories>
        </div>
    </div>
    <div class="row row-cols-1 row-cols-md-3 g-4 mt-3">
        <div class="col" v-for="{ spotId, spotImage, spotTitle, spotDescription, address } in spots.list" :key="spotId">
            <div class="card h-100">
                <img :src="spotImage" class="card-img-top" :alt="spotTitle">
                <div class="card-body">
                    <h5 class="card-title">{{ spotId }}-{{ spotTitle }}</h5>
                    <p class="card-text">{{ spotDescription.length <= 100 ? spotDescription : spotDescription.substring(0,
                        100) }}...</p>
                </div>
                <div class="card-footer">
                    <small class="text-body-secondary">{{ address }}</small>
                </div>
            </div>
        </div>

    </div>
    <!-- <nav class="mt-3">
        <ul class="pagination">
            <li class="page-item" v-for="(value, index) in totalPages" :key="index" @click="clickHandler(value)">
                <a :class="{ 'currentPage': datas.start === value, 'page-link': true }">{{ value }}</a>
            </li>
        </ul>
    </nav> -->
    <paging :totalPages="totalPages" :thePage="datas.start" @childClick="clickHandler"></paging>
</template>
    
<script setup>
import axios from 'axios'
import { onMounted, reactive, ref } from 'vue'
import Paging from '../components/Paging.vue'
import SearchTextBox from '../components/SearchTextBox.vue'
import PageSize from '../components/PageSize.vue'
import SpotSort from '../components/SpotSort.vue'
import Categories from '../components/Categories.vue'

const spots = reactive({})
const datas = ref({ "categoryId": 0, "keyword": "", "start": 0, "rows": 0, "dir": false, "sort": "spotId" })
const totalPages = ref(0)
// const data = reactive({
//     categories: [],
// });


const loadSpots = async () => {

    const API_URL = `${import.meta.env.VITE_API_SPOTURL}/spotimages`
    const response = await axios.post(API_URL, datas.value)
    Object.assign(spots, response.data)

    //計算總共幾頁  
    totalPages.value = +datas.value.rows === 0 ? 1 : Math.ceil(response.data.count / datas.value.rows)
}

//分頁功能
const clickHandler = page => {
    datas.value.start = page
    loadSpots()
}
//關鍵字搜尋
const inputHandler = value => {
    console.log("value")
    datas.value.keyword = value
    datas.value.start = 1
    loadSpots()
}
//設定每頁幾筆資料
const changeHandler = value => {
    datas.value.rows = +value
    datas.value.start = 1
    loadSpots()
}

//排序
const sortChangeHandler = value => {
    datas.value.dir = value.dir
    datas.value.sort = value.sort
    loadSpots()
}

//根據景點分類帶出產品資料
const categoryClickHandler = id => {
    datas.value.categoryId = id
    datas.value.start = 1
    loadSpots()
}

onMounted(async () => {
    loadSpots();
})

</script>
    
<style scoped></style>