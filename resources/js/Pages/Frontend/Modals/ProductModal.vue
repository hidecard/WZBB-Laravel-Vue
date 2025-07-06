<script setup>
const props = defineProps({
    product: Object,
})

import { nextTick, watch, ref } from 'vue';
import { tns } from "tiny-slider";
import "tiny-slider/dist/tiny-slider.css";

const images = ref([]);
watch(() => props.product, async (newProduct) => {
    images.value = JSON.parse(newProduct.image);

    await nextTick();
    initSlider();
});

function initSlider() {
    tns({
        container: '#productModal',
        items: 1,
        navContainer: images.value.length > 1 ? '#productModalThumbnails' : null,
        navAsThumbnails: images.value.length > 1,
        speed: 1500,
    });
}


import { addToWishList, addToCart } from '../Components/Utils/CartWishManage';
const quantity = ref(1);
function quantitySelect(incOrDec) {
    if (incOrDec) quantity.value++;
    else quantity.value--
}
</script>

<template>
    <!-- Modal -->
    <div class="modal fade" id="productViewModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-xl modal-dialog-centered">
            <div class="modal-content">
                <div class="p-8 modal-body">
                    <div class="top-0 mt-3 position-absolute end-0 me-3">
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="row" id="productData">

                        <div class="col-lg-6">
                            <!-- img slide -->
                            <div class="product productModal" id="productModal">
                                <div v-for="(image, index) in images" :key="index">
                                    <div>
                                        <!-- img -->
                                        <img :src="'/images/products/' + image" alt="" />
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="mt-6 ps-lg-8 mt-lg-0">
                                <a href="#" class="mb-4 d-block" id="categoryTitle">{{ product.categoryName }}</a>
                                <h2 class="mb-1 h1" id="productTitle">{{ product.name }}
                                </h2>
                                <div class="mb-4">
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
                                    <span :show="product.sale_price" class="text-dark me-2">{{
                                        product.sale_price }}
                                    </span>
                                    <span
                                        :class="product.sale_price ? 'text-decoration-line-through text-muted' : 'text-dark'">{{
                                            product.price
                                        }}
                                    </span>
                                    <span>
                                        <small class="fs-6 ms-2 text-danger">26% Off</small>
                                    </span>
                                </div>

                                <hr class="my-6" />
                                <div class="mb-4">
                                    <button type="button" class="btn btn-outline-secondary">250g</button>
                                    <button type="button" class="btn btn-outline-secondary">500g</button>
                                    <button type="button" class="btn btn-outline-secondary">1kg</button>
                                </div>
                                <div>
                                    <!-- input -->
                                    <!-- input -->
                                    <div class="input-group input-spinner">
                                        <input @click="quantitySelect(0)" type="button" value="-"
                                            class="button-minus btn btn-sm" data-field="quantity" />
                                        <input v-model="quantity" id="quantity" type="text" name="quantity"
                                            class="quantity-field form-control-sm form-input" />
                                        <input @click="quantitySelect(1)" type="button" value="+"
                                            class="button-plus btn btn-sm" data-field="quantity" />
                                    </div>
                                </div>
                                <div class="mt-3 row justify-content-start g-2 align-items-center">
                                    <div class="col-lg-4 col-md-5 col-6 d-grid">
                                        <!-- button -->
                                        <!-- btn -->
                                        <button @click="addToCart(product, quantity)" type="button"
                                            class="btn main-theme">
                                            <i class="feather-icon icon-shopping-bag me-2"></i>
                                            Add to cart
                                        </button>
                                    </div>
                                    <div class="col-md-4 col-5">
                                        <!-- btn -->
                                        <a @click="addToWishList(product.id)" class="btn btn-light" href="#!"
                                            data-bs-toggle="tooltip" data-bs-html="true" aria-label="Wishlist">
                                            <i class="feather-icon icon-heart"></i></a>
                                    </div>
                                </div>
                                <hr class="my-6" />
                                <div>
                                    <table class="table table-borderless">
                                        <tbody>

                                           
                                            <tr>
                                                <td>Type:</td>
                                                <td>Fruits</td>
                                            </tr>
                                            <tr>
                                                <td>Shipping:</td>
                                                <td>
                                                    <small>
                                                        01 day shipping.
                                                        <span class="text-muted">( Free
                                                            pickup today)</span>
                                                    </small>
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
