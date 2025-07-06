<script setup>
import { ref, watch } from 'vue';
import { useForm } from '@inertiajs/vue3';
import MasterFrontend from './Layout/MasterFrontend.vue';
import CheckoutCart from './Components/CheckoutCart.vue';
defineOptions({
    layout: MasterFrontend,
})

const props = defineProps({
    user : Object,
    homeAdress : Object,
    officeAddress : Object,
    paymentOptions : Array
})

import AddressModal from './Components/AddressModal.vue';
import { toast } from 'vue3-toastify';
const showModal = ref(false);
const newProps = ref('');

const openModel =  (types) => {
    if (types == 'new') newProps.value = {};
    else if (types == 'editHome') newProps.value = props.homeAddress;
    else newProps.value = props.officeAddress;

    showModal.value = true;
};


let updateAddress = useForm({});

function updateDefault(){
    updateAddress.post(route('addressDefault.update'), {
        onSuccess: () => {
            toast.success("Address Updated");
        },
        onError: () => {
            toast.error(updateAddress.errors.message);
        },
    })
}

function deleteAddress(id) {
    useForm({}).get(route('address.delete', id))
}


const isAddress = ref(props.homeAddress?.is_default || props.officeAddress?.is_default);



</script>
<template>
    <main>
        <!-- section -->
        <section class="mt-8 mb-8 mb-lg-14">
            <div class="container">
                    <h1 class="mb-3 fw-bold">Checkout</h1>
                <div>
                    <!-- row -->
                    <div class="row">
                        <div class="col-xl-7 col-lg-6 col-md-12">
                            <!-- accordion -->
                            <div class="accordion accordion-flush" id="accordionFlushExample">
                                <!-- accordion item -->
                                <div class="py-4 accordion-item">
                                    <div class="d-flex justify-content-between align-items-center">
                                        <!-- heading one -->
                                        <a href="#" class="fs-5 text-inherit collapsed h4" data-bs-toggle="collapse"
                                            data-bs-target="#flush-collapseOne" aria-expanded="true"
                                            aria-controls="flush-collapseOne">
                                            <i class="feather-icon icon-map-pin me-2 text-muted"></i>
                                            Add delivery address
                                        </a>
                                        <!-- btn -->
                                        <a @click="openModal('new')" class="btn btn-outline-primary btn-sm">Add a new
                                            address</a>
                                        <!-- collapse -->
                                    </div>
                                    <div id="flush-collapseOne" class="accordion-collapse collapse show"
                                        data-bs-parent="#accordionFlushExample">
                                        <div class="mt-5">
                                            <div class="row">
                                                <!-- address will goes here  -->
                                            </div>
                                        </div>
                                    </div>

                                </div>


                                <!-- accordion item -->
                                <div class="py-4 accordion-item">
                                    <a href="#" class="text-inherit h5" data-bs-toggle="collapse"
                                        data-bs-target="#flush-collapseFour" aria-expanded="false"
                                        aria-controls="flush-collapseFour">
                                        <i class="feather-icon icon-credit-card me-2 text-muted"></i>
                                        Payment Method
                                        <!-- collapse -->
                                    </a>
                                    <div id="flush-collapseFour" class="accordion-collapse collapse"
                                        data-bs-parent="#accordionFlushExample">
                                        <div class="mt-5">
                                            <div class="">
                                                <div class="mb-2 shadow-none card card-bordered">
                                                    <!-- card body -->
                                                    <div class="p-6 card-body">
                                                        <!-- check input -->
                                                        <div class="d-flex">
                                                            <div class="form-check">
                                                                <input
                                                                    value="others" class="form-check-input" type="radio"
                                                                    name="flexRadioDefault" id="others" />
                                                                <label class="form-check-label ms-2"
                                                                    for="others"></label>
                                                            </div>
                                                            <div>
                                                                <!-- title -->
                                                                <h5 class="mb-1 h6">Pay with other Banks
                                                                </h5>
                                                                <p class="mb-0 small">You will be redirected to
                                                                    SSLCOMMERZ
                                                                    Gateway to complete your purchase securely.</p>
                                                            </div>
                                                        </div>
                                                    </div>
                                                </div>
                                                <!-- card -->
                                                <div class="shadow-none card card-bordered">
                                                    <div class="p-6 card-body">
                                                        <!-- check input -->
                                                        <div class="d-flex">
                                                            <div class="form-check">
                                                                <input value="cod"
                                                                    class="form-check-input" type="radio"
                                                                    name="flexRadioDefault" id="cashonDelivery" />
                                                                <label class="form-check-label ms-2"
                                                                    for="cashonDelivery"></label>
                                                            </div>
                                                            <div>
                                                                <!-- title -->
                                                                <h5 class="mb-1 h6">Cash on Delivery</h5>
                                                                <p class="mb-0 small">Pay with cash when your order is
                                                                    delivered.</p>
                                                            </div>
                                                        </div>
                                                    </div>
                                                </div>
                                                <!-- Button -->
                                                <div class="mt-5 d-flex justify-content-end">
                                                    <a href="#" class="btn btn-outline-gray-400 text-muted"
                                                        data-bs-toggle="collapse" data-bs-target="#flush-collapseThree"
                                                        aria-expanded="false" aria-controls="flush-collapseThree">
                                                        Prev
                                                    </a>
                                                    <button class="btn main-theme ms-2">Place Order</button>
                                                    <!-- payment method options will go here  -->
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <!-- <CheckoutCart />  will goes here-->
                    </div>
                </div>
            </div>
        </section>
    </main>

</template>
