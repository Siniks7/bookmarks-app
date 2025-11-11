<script setup lang="ts">
import { useRoute } from 'vue-router';
import type { Category } from '@/interfaces/category.inteface';
import { useCategoryStore } from '@/stores/categories.store';
import { ref, watch } from 'vue';
const route = useRoute();
const state = useCategoryStore();
const category = ref<Category>();

watch(
  () => ({
    alias: route.params.alias,
    categories: state.categories,
  }),
  (data) => {
    category.value = state.getCategoryByAlias(data.alias as string);
  },
);
</script>

<template>
  Category
  {{ category?.name }}
</template>
