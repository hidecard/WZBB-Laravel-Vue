<script setup>
import MasterBackend from './Layout/MasterBackend.vue';
import Pagination from './Components/Pagination.vue';

defineOptions({
    layout: MasterBackend
});

const props = defineProps({
    categories: Array
    
});

// Import necessary Vue and Inertia features
import { computed, ref } from 'vue';
import { toast } from 'vue3-toastify';
import { useForm } from '@inertiajs/vue3';

// Search & status filter
const search = ref('');

const filteredProducts = computed(() => {
    let newCategories = props.categories.data;

    if (search.value) {
        const searchText = search.value.toLowerCase();
        newCategories = newCategories.filter(category =>
            category.name.toLowerCase().includes(searchText)
        );
    }

    return newCategories;
});

// Delete category (using DELETE method)
function deleteCategory(category_id) {
    if (confirm("Are you sure you want to delete this category?")) {
        useForm({}).delete(route('dash.category.delete', category_id), {
            onSuccess: () => {
                toast.success('Category deleted successfully');
            },
            onError: () => {
                toast.error('Failed to delete Category');
            }
        });
    }
}
</script>

<template>
    <main class="main-content-wrapper">
        <div class="container">
            <!-- Header row -->
            <div class="mb-8 row">
                <div class="col-md-12">
                    <div class="gap-4 d-flex flex-column flex-md-row justify-content-between align-items-md-center">
                        <!-- Page header -->
                        <div>
                            <h2>Categories</h2>
                            <nav aria-label="breadcrumb">
                                <ol class="mb-0 breadcrumb">
                                    <li class="breadcrumb-item">
                                        <Link :href="route('dashboard')" class="text-inherit">Dashboard</Link>
                                    </li>
                                    <li class="breadcrumb-item active" aria-current="page">Categories</li>
                                </ol>
                            </nav>
                        </div>
                        <!-- Add new button -->
                        <div>
                            <a :href="route('dash.category.add')" class="btn main-theme">Add New Category</a>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Table row -->
            <div class="row">
                <div class="mb-5 col-xl-12 col-12">
                    <div class="card h-100 card-lg">
                        <div class="px-6 py-6">
                            <div class="row justify-content-between">
                                <div class="mb-2 col-lg-4 col-md-6 col-12 mb-md-0">
                                    <form class="d-flex" role="search">
                                        <input v-model="search" class="form-control" type="search"
                                            placeholder="Search Category" aria-label="Search" />
                                    </form>
                                </div>
                                <div class="col-xl-2 col-md-4 col-12">
                                    <select v-model="selectedStatus" class="form-select">
                                        <option value="">All Status</option>
                                        <option value="1">Active</option>
                                        <option value="0">Deactive</option>
                                    </select>
                                </div>
                            </div>
                        </div>

                        <!-- Table body -->
                        <div class="p-0 card-body">
                            <div class="table-responsive">
                                <table class="table mb-0 text-center table-centered table-hover text-nowrap table-borderless table-with-checkbox">
                                    <thead class="bg-light">
                                        <tr>
                                            <th>Icon</th>
                                            <th>Name</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="category in filteredProducts" :key="category.id">
                                            <td>
                                                <a href="#">
                                                    <img :src="`/images/categories/${category.image}`" alt=""
                                                        class="icon-shape icon-md" />
                                                </a>
                                            </td>
                                            <td>{{ category.name }}</td>
                                            <td>
                                                <div class="dropdown">
                                                    <a href="#" class="text-reset" data-bs-toggle="dropdown" aria-expanded="false">
                                                        <i class="feather-icon icon-more-vertical fs-5"></i>
                                                    </a>
                                                    <ul class="dropdown-menu">
                                                        <li>
                                                            <a class="dropdown-item" @click="deleteCategory(category.id)">
                                                                <i class="bi bi-trash me-3"></i>
                                                                Delete
                                                            </a>
                                                        </li>
                                                        <li>
                                                            <Link class="dropdown-item" :href="route('dash.category.editPage', category.id)">
                                                                <i class="bi bi-pencil-square me-3"></i>
                                                                Edit
                                                            </Link>
                                                        </li>
                                                    </ul>
                                                </div>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>

                        <!-- Pagination -->
                        <Pagination :paginator="categories" :scrollPreserve="false" />
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>
