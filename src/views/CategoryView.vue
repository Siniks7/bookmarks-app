<script setup lang="ts">
import { onBeforeRouteUpdate, useRoute } from 'vue-router';
import type { Category } from '@/interfaces/category.inteface';
import { useCategoryStore } from '@/stores/categories.store';
import { ref, watch } from 'vue';
const route = useRoute();
const state = useCategoryStore();
const category = ref<Category>();

watch(
  () => state.categories,
  () => {
    category.value = state.getCategoryByAlias(route.params.alias as string);
  },
);

onBeforeRouteUpdate((to) => {
  category.value = state.getCategoryByAlias(to.params.alias as string);
});
</script>

<template>
  Category
  {{ category?.name }}
</template>
