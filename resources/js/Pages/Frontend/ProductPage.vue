<script setup>
import MasterFrontend from './Layout/MasterFrontend.vue';
import ProductComponent from './Components/ProductComponent.vue';
defineOptions({
    layout: MasterFrontend,
})
const props = defineProps({
    product: Array
})

import { onBeforeMount, onMounted, ref } from 'vue';
const images = ref([]);
onBeforeMount(() => {
    images.value = JSON.parse(props.product.image);
});

import { addToWishList, addToCart } from './Components/Utils/CartWishManage';
const quantity = ref(1);
function quantitySelect(incOrDec) {
    if (incOrDec) quantity.value++;
    else quantity.value--
}
</script>

<template>

    <Head>
        <meta head-key="og:title" property="og:title" :content="product.meta_title">
        <title>{{ product.name }} - FreshCart</title>
    </Head>

    <main>
        <section class="mt-8">
            <div class="container">
                <div class="row">
                    <div class="col-md-5 col-xl-6">
                        <!-- img slide -->
                        <div class="product" id="product">
                            <div v-for="(image, index) in images" :key="index">
                                <div class="d-flex justify-content-center">
                                    <!-- img -->
                                    <img class="w-50" :src="'/images/products/' + image" alt="" />
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-7 col-xl-6">
                        <div class="mt-6 ps-lg-10 mt-md-0">
                            <!-- content -->
                            <Link :href="route('category.view', [product.category.slug])" class="mb-4 d-block">{{
                                product.category.name }}</Link>
                            <!-- heading -->
                            <h1 class="mb-1">{{ product.name }}</h1>
                            <div class="mb-4">
                                <!-- rating -->
                                <!-- rating -->
                                <small class="text-warning">
                                    <i class="bi bi-star-fill"></i>
                                    <i class="bi bi-star-fill"></i>
                                    <i class="bi bi-star-fill"></i>
                                    <i class="bi bi-star-fill"></i>
                                    <i class="bi bi-star-half"></i>
                                </small>
                                <a href="#" class="ms-2">(30 reviews)</a>
                            </div>
                            <div class="fs-4">
                                <!-- price -->
                                <span class="fw-bold text-dark">{{ product.sale_price }}</span>
                                <span class="text-decoration-line-through text-muted">{{ product.price }}</span>
                                <span><small class="fs-6 ms-2 text-danger">26% Off</small></span>
                            </div>
                            <!-- hr -->
                            <hr class="my-6" />
                            <div class="mb-5">
                                <button type="button" class="btn btn-outline-secondary">250g</button>
                                <!-- btn -->
                                <button type="button" class="btn btn-outline-secondary">500g</button>
                                <!-- btn -->
                                <button type="button" class="btn btn-outline-secondary">1kg</button>
                            </div>
                            <div>
                                <!-- input -->
                                <div class="input-group input-spinner">
                                    <input @click="quantitySelect(0)" type="button" value="-"
                                        class="button-minus btn btn-sm" data-field="quantity" />
                                    <input v-model="quantity" type="number" name="quantity"
                                        class="quantity-field form-control-sm form-input" />
                                    <input @click="quantitySelect(1)" type="button" value="+"
                                        class="button-plus btn btn-sm" data-field="quantity" />
                                </div>
                            </div>
                            <div class="mt-3 row justify-content-start g-2 align-items-center">
                                <div class="col-xxl-4 col-lg-4 col-md-5 col-5 d-grid">
                                    <!-- button -->
                                    <!-- btn -->
                                    <button @click="addToCart(product, quantity)" type="button" class="btn main-theme">
                                        <i class="feather-icon icon-shopping-bag me-2"></i>
                                        Add to cart
                                    </button>
                                </div>
                                <div class="col-md-4 col-4">
                                    <a @click="addToWishList(product.id)" class="btn btn-light" href="#"
                                        data-bs-toggle="tooltip" data-bs-html="true" aria-label="Wishlist"><i
                                            class="feather-icon icon-heart"></i></a>
                                </div>
                            </div>
                            <!-- hr -->
                            <hr class="my-6" />
                            <div>

                                <span>Shipping: </span>
                                <span>
                                    <small>
                                        01 day shipping.
                                        <span class="text-muted">( Free pickup today)</span>
                                    </small>
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

    </main>
</template>
