<script setup>
import Master from '../Master.vue'
defineOptions({
    layout: Master,
})
import { toast } from 'vue3-toastify';
import { useForm } from '@inertiajs/vue3';
const form = useForm({
    firstName: null,
    lastName: null,
    email: null,
    phone: null,
    password: null,
});

const submit = () => {
    form.post(route('user.register.verify'), {
        onSuccess: () => {
            sessionStorage.setItem('source', 'user.register.post');
            toast.success('Verify your email');
        },
        onError: () => {
            toast.error(form.errors.message);
        },
    });
}
</script>
<template>

    <Head>
        <title>FreshCart - SignUp</title>
    </Head>

    <div class="shadow-sm border-bottom">
        <nav class="py-2 navbar navbar-light">
            <div class="container justify-content-center justify-content-lg-between">
                <Link class="navbar-brand" :href="route('index')">
                <img :src="'images/yha_logo.png'" alt="" class="align-text-top d-inline-block w-25">
                </Link>
                <span class="navbar-text">
                    Already have an account?
                    <Link :href="route('user.login')" class="text-main" style="color: #ff6c0f;">Sign in</Link>

                </span>
            </div>
        </nav>
    </div>


    <!-- section -->
    <section class="my-8 my-lg-14">
        <!-- container -->
        <div class="container">
            <!-- row -->
            <div class="row justify-content-center align-items-center">
                <div class="order-2 col-12 col-md-6 col-lg-4 order-lg-1">
                    <!-- img -->
                    <img :src="'images/signup-g.svg'" alt="" class="img-fluid">
                </div>
                <!-- col -->
                <div class="order-1 col-12 col-md-6 offset-lg-1 col-lg-4 order-lg-2">
                    <div class="mb-5 mb-lg-9">
                        <h1 class="mb-1 h2 fw-bold">Get Start Shopping</h1>
                        <p>Welcome to FreshCart! Enter your email to get started.</p>
                    </div>
                    <!-- form -->
                    <form @submit.prevent="submit" class="row g-3">
                        <div class="row g-3">
                            <!-- col -->
                            <div class="col">
                                <!-- input --><input type="text" class="form-control" placeholder="First name"
                                    aria-label="First name" id="fname" v-model="form.firstName" required>
                                <small class=" text-danger">{{ form.errors.firstName }}</small>

                            </div>
                            <div class="col">
                                <!-- input --><input type="text" class="form-control" placeholder="Last name"
                                    v-model="form.lastName" aria-label="Last name" id="lname" required>
                                <small class=" text-danger">{{ form.errors.lastName }}</small>

                            </div>
                            <div class="col-12">

                                <!-- input --><input type="email" class="form-control" id="email" placeholder="Email"
                                    v-model="form.email" required>
                                <small class=" text-danger">{{ form.errors.email }}</small>

                            </div>
                            <div class="col-12">

                                <!-- input --><input type="text" class="form-control" id="phone"
                                    placeholder="Phone Number" maxlength="11" v-model="form.phone" required>
                                <small class=" text-danger">{{ form.errors.phone }}</small>

                            </div>
                            <div class="col-12">

                                <!-- input --><input type="password" class="form-control" id="password"
                                    placeholder="Password" v-model="form.password" required>
                                <small class=" text-danger">{{ form.errors.password }}</small>

                            </div>
                            <!-- btn -->
                            <div class="col-12 d-grid"> <button type="submit" class="btn main-theme">Register</button>
                            </div>

                            <!-- text -->
                            <p><small>By continuing, you agree to our <a href="#!"> Terms of Service</a> & <a
                                        href="#!">Privacy
                                        Policy</a></small></p>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </section>
</template>
