<script setup>
import Master from '../Master.vue'
defineOptions({
    layout: Master,
})

import { toast } from 'vue3-toastify';
import { useForm } from '@inertiajs/vue3';
const form = useForm({
    email: '',
    password: '',
    remember: false,
})

 const submit = () => {
    form.post(route('user.login.post'), {
        onSuccess: () => {
            toast.success("Login successful!");
        },
        onError: () => {
            toast.error(form.errors.message);
        },
    });
}
</script>
<template>

    <Head>
        <title>FreshCart - Login</title>
    </Head>
    <!-- navigation -->
    <div class="shadow-sm border-bottom">
        <nav class="py-2 navbar navbar-light">
            <div class="container justify-content-center justify-content-lg-between">
                <Link class="navbar-brand" :href="route('index')">
                <img :src="'images/freshcart-logo.svg'" alt="" class="align-text-top d-inline-block">
                </Link>
                <span class="navbar-text">
                    Don't have an account?
                    <Link :href="route('user.register')" class="text-primary link-success">Sign Up
                    </Link>

                </span>
            </div>
        </nav>
    </div>

    <!-- section -->
    <section class="my-8 my-lg-14">
        <div class="container">
            <!-- row -->
            <div class="row justify-content-center align-items-center">
                <div class="order-2 col-12 col-md-6 col-lg-4 order-lg-1">
                    <!-- img -->
                    <img :src="'images/signin-g.svg'" alt="" class="img-fluid">
                </div>
                <!-- col -->
                <div class="order-1 col-12 col-md-6 offset-lg-1 col-lg-4 order-lg-2">
                    <div class="mb-5 mb-lg-9">
                        <h1 class="mb-1 h2 fw-bold">Sign in to FreshCart</h1>
                        <p>Welcome back to FreshCart! Enter your email to get started.</p>
                    </div>

                    <div>
                        <form @submit.prevent="submit" class="row g-3">
                            <!-- row -->

                            <div class="col-12">
                                <!-- input -->
                                <input type="email" class="form-control" id="email" placeholder="Email"
                                    v-model="form.email" required>
                                <small class=" text-danger">{{ form.errors.name }}</small>
                            </div>
                            <div class="col-12">
                                <!-- input -->
                                <input type="password" class="form-control" id="password" placeholder="Password"
                                    v-model="form.password" required>
                                <small class=" text-danger">{{ form.errors.password }}</small>

                            </div>
                            <div class="d-flex justify-content-between">
                                <!-- form check -->
                                <div class="form-check">
                                    <input class="form-check-input" type="checkbox" v-model="form.remember"
                                        id="flexCheckDefault">
                                    <!-- label --> <label class="form-check-label" for="flexCheckDefault">
                                        Remember me
                                    </label>
                                </div>
                            </div>
                            <!-- btn -->
                            <div class="col-12 d-grid"> <button type="submit" class="btn main-theme">Sign In</button>
                            </div>
                            <!-- link -->
                            <div>Forgot your password?
                                <Link :href="route('user.resetpwd')" class="text-primary link-success">Reset Password
                                </Link>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>
