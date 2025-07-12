<script setup>
import { ref, watch } from 'vue';
import { useForm } from '@inertiajs/vue3';
import MasterFrontend from './Layout/MasterFrontend.vue';
import CheckoutCart from './Components/CheckoutCart.vue';
defineOptions({
    layout: MasterFrontend,
})


const props = defineProps({
    user: Object,
    homeAddress: Object,
    officeAddress: Object,
    paymentOptions: Array,
})

import AddressModal from './Components/AddressModal.vue';
const showModal = ref(false);
const newProps = ref('');
const openModal = (types) => {
    if (types == 'new') newProps.value = {};
    else if (types == 'editHome') newProps.value = props.homeAddress;
    else newProps.value = props.officeAddress;

    showModal.value = true;
};

let addressUpdate = useForm({});
function updateDefault() {
    addressUpdate.get(route('addressDefault.update'), {
        onSuccess: () => {
            toast.success("Default address updated");
        },
        onError: () => {
            toast.error(form.errors.message);
        },
    });
}

function deleteAddress(id) {
    useForm({}).get(route('address.delete', id))
}

//checkout methods
const isAddress = ref(props.homeAddress?.is_default || props.officeAddress?.is_default);
watch(() => [props.homeAddress, props.officeAddress], () => {
    isAddress.value = props.homeAddress?.is_default || props.officeAddress?.is_default;
});
const paymentMethod = ref(null);
const checkOutForm = useForm({
    products: JSON.parse(localStorage.getItem("cartList")).map(product => {
        return { id: product.id, quantity: product.quantity }
    }),
    payment_method: paymentMethod.value,
});

function checkout() {
    loader.show();
    checkOutForm.post(route('create.invoice'), {
        preserveScroll: true,
        onSuccess: () => {
            if (checkOutForm.payment_method == 'bkash') {
                props.paymentOptions.value = props.paymentOptions.desc;
                openPaymentModal();
            } else if (checkOutForm.payment_method == 'cod') {
                toast.success('Order Placed!');
                setTimeout(() => {
                    window.location.href = route('profile');
                }, 2000);
            } else {
                window.location.href = props.paymentOptions.GatewayPageURL;
            }
            checkOutForm.reset();
            localStorage.removeItem("cartList");
            props.paymentOptions = "";
        },
        onError: () => {
            toast.error(checkOutForm.errors.message);
        },
        onFinish: () => {
            loader.hide();
        },
    });
}


import PaymentOptionModal from './Components/PaymentOptionModal.vue';
const showPaymentModal = ref(false);
const openPaymentModal = () => {
    showPaymentModal.value = true;
};
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
                                                <div v-if="homeAddress" class="mb-4 col-xl-6 col-lg-12 col-md-6 col-12">
                                                    <!-- form -->
                                                    <div class="p-6 card card-body">
                                                        <div class="mb-4 form-check">
                                                            <input class="form-check-input" type="radio"
                                                                name="flexRadioDefault" id="homeRadio"
                                                                :checked="homeAddress.is_default" />
                                                            <label class="form-check-label text-dark"
                                                                for="homeRadio">Home
                                                            </label>
                                                        </div>
                                                        <!-- address -->
                                                        <address>
                                                            <strong>{{ user.firstName + ' ' + user.lastName }}</strong>
                                                            <br />
                                                            {{ homeAddress.address }},
                                                            <br />
                                                            {{ homeAddress.district }}, {{ homeAddress.city }}, {{
                                                                homeAddress.country }},
                                                            <br />

                                                            <abbr title="Phone">Phone: {{ user.phone }}</abbr>
                                                        </address>

                                                        <button @click="updateDefault" v-if="!homeAddress.is_default"
                                                            class="btn btn-outline-danger btn-sm col-6">Make Default
                                                            address </button>
                                                        <div class="mt-4">
                                                            <a @click="openModal('editHome')"
                                                                class="text-inherit">Edit</a>
                                                            <a @click="deleteAddress(homeAddress.id)"
                                                                class="text-danger ms-3">Delete</a>
                                                        </div>
                                                    </div>
                                                </div>
                                                <div v-if="officeAddress"
                                                    class="mb-4 col-xl-6 col-lg-12 col-md-6 col-12">
                                                    <!-- input -->
                                                    <div class="p-6 card card-body">
                                                        <div class="mb-4 form-check">
                                                            <input class="form-check-input" type="radio"
                                                                name="flexRadioDefault" id="officeRadio"
                                                                :checked="officeAddress.is_default" />
                                                            <label class="form-check-label text-dark"
                                                                for="officeRadio">Office </label>

                                                        </div>
                                                        <address>
                                                            <strong>{{ user.firstName + ' ' + user.lastName }}</strong>
                                                            <br />
                                                            {{ officeAddress.address }},
                                                            <br />
                                                            {{ officeAddress.district }}, {{ officeAddress.city }}, {{
                                                                officeAddress.country }},
                                                            <br />

                                                            <abbr title="Phone">Phone: {{ user.phone }}</abbr>
                                                        </address>
                                                        <button @click="updateDefault" v-if="!officeAddress.is_default"
                                                            class="btn btn-outline-danger btn-sm col-6">Make Default
                                                            address</button>
                                                        <div class="mt-4">
                                                            <a @click="openModal('editOffice')"
                                                                class="text-inherit">Edit</a>
                                                            <a @click="deleteAddress(officeAddress.id)"
                                                                class="text-danger ms-3">Delete</a>
                                                        </div>

                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <AddressModal :address="newProps" v-if="showModal" @close="showModal = false" />
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
                                                                <input v-model="checkOutForm.payment_method"
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
                                                                <input v-model="checkOutForm.payment_method" value="cod"
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
                                                    <button
                                                        :disabled="checkOutForm.payment_method == null || !isAddress"
                                                        @click="checkout" class="btn main-theme ms-2">Place
                                                        Order</button>

                                                    <PaymentOptionModal :paymentOptions="paymentOptions"
                                                        v-if="showPaymentModal" @close="showPaymentModal = false" />
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <CheckoutCart />
                    </div>
                </div>
            </div>
        </section>
    </main>

</template>
