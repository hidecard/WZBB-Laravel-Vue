<script setup>
import { onMounted, reactive, ref, watch } from 'vue';
import MasterBackend from './Layout/MasterBackend.vue';
import { useForm } from '@inertiajs/vue3';
import { toast } from 'vue3-toastify';

defineOptions({
    layout: MasterBackend
});

const props = defineProps({
    product: Object,
    categories: Array
});

const editProductForm = useForm({
    name: props.product.name,
    category_id: props.product.category_id,
    weight: props.product.weight,
    price: props.product.price,
    sale_price: props.product.sale_price,
    tag: props.product.tag || "",
    status: props.product.status == 1,
    slug: props.product.slug,
    images: [],
});

const currentImages = JSON.parse(props.product.image);
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
    editProductForm.images = Array.from(event.target.files);
}

const submit = () => {
    editProductForm.post(route('dash.product.edit', props.product.id), {
        forceFormData: true,
        onSuccess: () => {
            toast.success('Product Updated');
        },
        onError: () => {
            if (editProductForm.errors.message)
                toast.error(editProductForm.errors.message);
        },
    });
};
</script>

<template>
    <main class="main-content-wrapper">
        <div class="container">
            <!-- Page Header -->
            <div class="mb-8 row">
                <div class="col-md-12">
                    <div class="d-md-flex justify-content-between align-items-center">
                        <div>
                            <h2>Edit Product</h2>
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
                                            v-model="editProductForm.name" />
                                        <div class="text-danger">{{ editProductForm.errors.name }}</div>
                                    </div>

                                    <!-- Category -->
                                    <div class="mb-3 col-lg-6">
                                        <label class="form-label">Product Category</label>
                                        <select v-model="editProductForm.category_id" class="form-select"
                                            name="category_id">
                                            <option disabled value="">Product Category</option>
                                            <option v-for="category in categories" :key="category.id"
                                                :value="category.id">
                                                {{ category.name }}
                                            </option>
                                        </select>
                                        <div class="text-danger">{{ editProductForm.errors.category_id }}</div>
                                    </div>

                                    <!-- Weight -->
                                    <div class="mb-3 col-lg-6">
                                        <label class="form-label">Weight</label>
                                        <input type="text" class="form-control" placeholder="Weight" name="weight"
                                            v-model="editProductForm.weight" />
                                        <div class="text-danger">{{ editProductForm.errors.weight }}</div>
                                    </div>

                                    <!-- Slug -->
                                    <div class="mb-3 col-lg-6">
                                        <label class="form-label">Slug</label>
                                        <input type="text" class="form-control" placeholder="Slug" name="slug"
                                            v-model="editProductForm.slug" />
                                        <div class="text-danger">{{ editProductForm.errors.slug }}</div>
                                    </div>

                                    <!-- Status -->
                                    <div class="mb-3 col-lg-12">
                                        <label class="form-label">Status</label>
                                        <div class="form-check form-switch">
                                            <input v-model="editProductForm.status" class="form-check-input"
                                                type="checkbox" name="status" id="flexCheckDefault" />
                                            <label class="form-check-label" for="flexCheckDefault">Active</label>
                                        </div>
                                    </div>

                                    <!-- Product Images -->
                                    <div class="mt-5 mb-3 col-lg-12">
                                        <h4 class="mb-3 h5">Product Images</h4>
                                        <div class="flex-wrap gap-2 d-flex" id="previewImg">
                                            <template v-if="newImages.length">
                                                <img v-for="(image, index) in newImages" :key="'new-' + index" :src="image"
                                                    class="w-25 h-25" />
                                            </template>
                                            <template v-else>
                                                <img v-for="(image, index) in currentImages"
                                                    :key="'current-' + index"
                                                    :src="'/images/products/' + image"
                                                    class="w-25 h-25" />
                                            </template>
                                        </div>
                                        <label for="attachment"
                                            class="p-3 mt-3 text-center border border-2 rounded form-label d-col"
                                            style="border-style: dashed!important;">
                                            <img :src="'/images/cloud-upload-svgrepo.svg'" class="w-25" alt="">
                                            <div>Attach Media</div>
                                        </label>
                                        <input @input="handleFileInput" type="file" accept="image/*" name="image[]"
                                            id="attachment" class="col-md-5 col-sm-10 d-none" multiple>
                                    </div>

                                    <!-- Product Price, Sale Price, Tag -->
                                    <div class="mt-5 mb-3 col-lg-12">
                                        <div class="mb-3">
                                            <label class="form-label">Product Price</label>
                                            <input type="text" class="form-control" placeholder="Enter Product Price"
                                                name="price" v-model="editProductForm.price" />
                                            <div class="text-danger">{{ editProductForm.errors.price }}</div>
                                        </div>

                                        <div class="mb-3">
                                            <label class="form-label">Sale Price</label>
                                            <input type="text" class="form-control" placeholder="Enter Sale Price"
                                                name="sale_price" v-model="editProductForm.sale_price" />
                                            <div class="text-danger">{{ editProductForm.errors.sale_price }}</div>
                                        </div>

                                        <div class="mb-3">
                                            <label class="form-label">Tag</label>
                                            <select v-model="editProductForm.tag" class="form-select" name="tag">
                                                <option value="">Select Tag</option>
                                                <option value="Hot">Hot</option>
                                                <option value="Sale">Sale</option>
                                            </select>
                                            <div class="text-danger">{{ editProductForm.errors.tag }}</div>
                                        </div>
                                    </div>

                                    <!-- Submit Button -->
                                    <div class="mb-3 col-12">
                                        <button type="submit" class="btn main-theme">Update Product</button>
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
