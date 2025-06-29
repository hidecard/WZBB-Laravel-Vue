<script setup>
import { onMounted, reactive, ref, watch } from 'vue';
import MasterBackend from './Layout/MasterBackend.vue';
import { useForm } from '@inertiajs/vue3';
defineOptions({
    layout: MasterBackend
})
const props = defineProps({
    categories: Array
});

const createProductForm = useForm({
    name: null,
    category_id: "",
    weight: null,
    price: null,
    sale_price: null,
    tag: "",
    status: true,
    slug: null,
    images: [],
});

const submit = () => {
    // console.log(route('dash.product.add'))
    createProductForm.post(route('dash.product.add'), {
        onSuccess: () => {
            toast.success('Product Created');
            createProductForm.reset();
            newImages.value = [];

        },
        onError: (error) => {
            console.log("Error response:", error);
            if (createProductForm.errors.message) {
                toast.error(createProductForm.errors.message);
            }
        },
    });
}


//parsing image file names to display current images and handling new image input and previewing them based on condition
const newImages = ref([]);
function handleFileInput(event) {
    newImages.value = [];
    const files = event.target.files;
    for (let i = 0; i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = () => {
            newImages.value.push(reader.result);
        };
        reader.readAsDataURL(files[i]);
    }
    //fills array with input images
    createProductForm.images = Array.from(event.target.files);
}

</script>

<template>
    <main class="main-content-wrapper">
        <div class="container">
            <!-- Page Header -->
            <div class="mb-8 row">
                <div class="col-md-12">
                    <div class="d-md-flex justify-content-between align-items-center">
                        <div>
                            <h2>Add Product</h2>
                            <nav aria-label="breadcrumb">
                                <ol class="mb-0 breadcrumb">
                                    <li class="breadcrumb-item">
                                        <Link :href="route('dashboard')" class="text-inherit">Dashboard</Link>
                                    </li>
                                    <li class="breadcrumb-item">
                                        <Link :href="route('dash.products')" class="text-inherit">Products</Link>
                                    </li>
                                    <li class="breadcrumb-item active" aria-current="page">Edit Product</li>
                                </ol>
                            </nav>
                        </div>
                        <div>
                            <Link :href="route('dash.products')" class="btn btn-light">Back to Product</Link>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Product Form -->
            <div class="row">
                <div class="col-lg-8 col-12">
                    <div class="mb-6 card card-lg">
                        <div class="p-6 card-body">
                            <h4 class="mb-4 h5">Product Information</h4>
                            <form @submit.prevent="submit">
                                <div class="row">
                                    <!-- Title -->
                                    <div class="mb-3 col-lg-6">
                                        <label class="form-label">Title</label>
                                        <input type="text" class="form-control" placeholder="Product Name" name="name"
                                            v-model="createProductForm.name" />
                                        <div class="text-danger">{{ createProductForm.errors.name }}</div>

                                    </div>

                                    <!-- Category -->
                                    <div class="mb-3 col-lg-6">
                                        <label class="form-label">Product Category</label>
                                        <select v-model="createProductForm.category_id" class="form-select"
                                            name="category_id">
                                            <option value="">Product Category</option>
                                            <option v-for="category in categories" :key="category.id"
                                                :value="category.id">
                                                {{ category.name }}
                                            </option>
                                        </select>
                                        <div class="text-danger">{{ createProductForm.errors.category }}</div>

                                    </div>


                                    <!-- Weight -->
                                    <div class="mb-3 col-lg-6">
                                        <label class="form-label">Weight</label>
                                        <input type="text" class="form-control" placeholder="Weight" name="weight"
                                            v-model="createProductForm.weight" />
                                        <div class="text-danger">{{ createProductForm.errors.weight }}</div>

                                    </div>
                                    <div class="mb-3 col-lg-6">
                                        <label class="form-label">Slug</label>
                                        <input type="text" class="form-control" placeholder="Slug" name="slug"
                                            v-model="createProductForm.slug" />
                                        <div class="text-danger">{{ createProductForm.errors.slug }}</div>

                                    </div>

                                    <div class="mb-3 col-lg-12">
                                        <label class="form-label">Status</label>
                                        <div class="form-check form-switch">
                                            <input v-model="createProductForm.status" class="form-check-input"
                                                type="checkbox" name="status" id="flexCheckDefault" />
                                            <label class="form-check-label" for="flexCheckDefault">Active</label>
                                        </div>
                                    </div>

                                    <!-- Product Images -->
                                    <div class="mt-5 mb-3 col-lg-12">
                                        <h4 class="mb-3 h5">Product Images</h4>
                                        <div class="gap-2 d-flex" id="previewImg">
                                            <img v-for="(image, index) in newImages" :key="index" :src="image"
                                                class="w-25 h-25" />
                                        </div>
                                        <label for="attachment"
                                            class="p-3 mt-3 text-center border border-2 rounded form-label d-col"
                                            style="border-style: dashed!important;">
                                            <img :src="'/images/cloud-upload-svgrepo.svg'" class="w-25" alt="">
                                            <div>Attach Media</div>
                                        </label>
                                        <input @input="handleFileInput" type="file" accept="image/*" name="image[]"
                                            id="attachment" class="col-md-5 col-sm-10 d-none" multiple>
                                        <div class="text-danger">{{ ' need to fix' }}</div>


                                    </div>

                                    <!-- Product Price, Sale Price, Quantity, Code, SKU -->
                                    <div class="mt-5 mb-3 col-lg-12">
                                        <div class="mb-3">
                                            <label class="form-label">Product Price</label>
                                            <input type="text" class="form-control" placeholder="Enter Product Price"
                                                name="price" v-model="createProductForm.price" />
                                            <div class="text-danger">{{ createProductForm.errors.price }}</div>

                                        </div>
                                        <div class="mb-3">
                                            <label class="form-label">Sale Price</label>
                                            <input type="text" class="form-control" placeholder="Enter Sale Price"
                                                name="sale_price" v-model="createProductForm.sale_price" />
                                            <div class="text-danger">{{ createProductForm.errors.sale_price }}</div>

                                        </div>

                                        <div class="mb-3">
                                            <label class="form-label">Tag</label>
                                            <select v-model="createProductForm.tag" class="form-select" name="tag">
                                                <option value="">Select Tag</option>
                                                <option value="Hot">Hot</option>
                                                <option value="Sale">Sale</option>
                                            </select>
                                            <div class="text-danger">{{ createProductForm.errors.tag }}</div>

                                        </div>
                                    </div>

                                    <!-- Submit Button -->
                                    <div class="mb-3 col-12">
                                        <button type="submit" class="btn main-theme">Create Product</button>
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>
