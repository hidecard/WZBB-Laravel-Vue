<script setup>
import { ref, watch } from "vue";
const totalAmmount = ref(0);
const cartList = ref([]);

cartList.value = localStorage.getItem("cartList")
    ? JSON.parse(localStorage.getItem("cartList"))
    : [];

cartList.value.forEach((product) => {
    totalAmmount.value = Number(
        (totalAmmount.value + product.price * product.quantity).toFixed(
            2
        )
    ); 
});


</script>
<template>
    <div class="col-md-12 offset-xl-1 col-xl-4 col-lg-6">
        <div class="mt-4 mt-lg-0">
            <div class="shadow-sm card">
                <h5 class="px-6 py-4 mb-0 bg-transparent">Order Details</h5>
                <ul class="list-group list-group-flush">
                    <!-- list group item -->
                    <li v-for="product in cartList" :key="product.id" class="px-4 py-3 list-group-item">
                        <div class="row align-items-center">
                            <div class="col-2 col-md-2">
                                <img :src="'/images/products/' + product.image" alt="Ecommerce" class="img-fluid" />
                            </div>
                            <div class="col-5 col-md-5">
                                <h6 class="mb-0">{{ product.name }}</h6>
                                <span><small class="text-muted">.98 / lb</small></span>
                            </div>
                            <div class="text-center col-2 col-md-2 text-muted">
                                <span>{{ product.quantity }}</span>
                            </div>
                            <div class="col-3 text-lg-end text-start text-md-end col-md-3">
                                <span class="fw-bold">{{ product.price * product.quantity }}</span>
                            </div>
                        </div>
                    </li>
                    <!-- list group item -->
                    <li class="px-4 py-3 list-group-item">
                        <div class="mb-2 d-flex align-items-center justify-content-between">
                            <div>Item Subtotal</div>
                            <div class="fw-bold">{{ totalAmmount }}</div>
                        </div>
                        <div class="d-flex align-items-center justify-content-between">
                            <div>
                                Service Fee
                                <i class="feather-icon icon-info text-muted" data-bs-toggle="tooltip"
                                    title="Default tooltip"></i>
                            </div>
                            <div class="fw-bold">$3.00</div>
                        </div>
                    </li>
                    <!-- list group item -->
                    <li class="px-4 py-3 list-group-item">
                        <div class="d-flex align-items-center justify-content-between fw-bold">
                            <div>Subtotal</div>
                            <div>{{ totalAmmount + 3 }}</div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
